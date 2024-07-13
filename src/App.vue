<template>
  <div>
    <h2>Operaciones con Producto Rest API</h2>
    <LinkButton iconCls="icon-add" @click="addRow()" style="width:80px;margin-bottom:4px">Adicionar</LinkButton>
    <LinkButton iconCls="icon-edit" @click="editRow()" style="width:80px;margin-bottom:4px">Editar</LinkButton>
    <LinkButton iconCls="icon-delete" @click="deleteRow()" style="width:80px;margin-bottom:4px">Eliminar</LinkButton>
    <LinkButton iconCls="icon-search" @click="fn_buscar()" style="width:80px;margin-bottom:4px">Buscar</LinkButton>
    <LinkButton iconCls="icon-undo" @click="fn_limpiar()" style="width:80px;margin-bottom:4px">Limpiar</LinkButton>
    <LinkButton iconCls="icon-refresh" @click="fetchData()" style="width:80px;margin-bottom:4px">Actualizar</LinkButton>
    <LinkButton iconCls="icon-home" @click="fn_principal()" style="width:80px;margin-bottom:4px">Principal</LinkButton>
    <DataGrid ref="datagrid"
              :data="data" style="height:450px; width:850px"
              :columnResizing="true"
              :filterable="filterable"
              :pagination="true"
              :selectionMode="selectionMode"
              @selectionChange="selection=$event">
      <GridColumn align="center" cellCss="datagrid-td-rownumber" width="30">
        <template v-slot:body="scope">
          {{ scope.rowIndex + 1 }}
        </template>
      </GridColumn>
      <GridColumn field="id" title="Id" filterable width=5%></GridColumn>
      <GridColumn field="name" title="Nombre Producto" filterable></GridColumn>
      <GridColumn field="description" title="Descripción" filterable></GridColumn>
      <GridColumn field="price" title="Precio" align="right" filterable width=15%>
        <template v-slot:body="scope">
          {{ formatPrice(scope.row.price) }}
        </template>
      </GridColumn>
    </DataGrid>

    <Dialog ref="dlg" bodyCls="f-column" :title="title" :modal="true" closed :dialogStyle="{height:'311px'}">
      <div class="f-full" style="overflow:auto">
        <Form ref="form" :model="model" :rules="rules" @validate="errors=$event" style="padding:5px 25px">
          <div style="margin-bottom:20px">
            <Label for="id">ID:</Label>
            <TextBox inputId="id" name="id" v-model="model.id"></TextBox>
            <div class="error">{{ getError('id') }}</div>
          </div>
          <div style="margin-bottom:20px">
            <Label for="name">Nombre:</Label>
            <TextBox inputId="name" name="name" v-model="model.name"></TextBox>
            <div class="error">{{ getError('name') }}</div>
          </div>
          <div style="margin-bottom:20px">
            <Label for="description">Descripcion:</Label>
            <TextBox inputId="description" name="description" v-model="model.description"></TextBox>
            <div class="error">{{ getError('description') }}</div>
          </div>
          <div style="margin-bottom:20px">
            <Label for="price">Precio:</Label>
            <NumberBox inputId="price" name="price" :precision="2" v-model="model.price"></NumberBox>
          </div>
        </Form>
      </div>
      <div class="buttons f-noshrink">
        <LinkButton iconCls="icon-ok" @click="saveRow()">Grabar</LinkButton>
        <LinkButton iconCls="icon-cancel" @click="$refs.dlg.close()">Cancelar</LinkButton>
      </div>
    </Dialog>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ProductOperations',
  data() {
    return {
      data: [],
      title: '',
      model: {
        id: null,
        name: null,
        description: null,
        price: null
      },
      rules: {
        name: 'required',
        description: 'required',
      },
      errors: {},
      editingRow: null,
      selectionOptions: [
        { value: null, text: "None" },
        { value: "single", text: "Single Selection" },
        { value: "multiple", text: "Multiple Selection" },
        { value: "cell", text: "Cell Selection" },
        { value: "multicell", text: "Multiple Cells" }
      ],
      selectionMode: "single",
      selection: null,
      filterable: true
    };
  },
  mounted() {  

    document.title = "Productos";
    this.fetchData();    
  },
  methods: {
    formatPrice(value) {
      return new Intl.NumberFormat('es-BO', { style: 'currency', currency: 'BOB' }).format(value);
      
    },
    fetchData() {
      axios.get('http://192.168.1.10/aprod/')
        .then(response => {
          this.data = response.data;
        })
        .catch(error => {
          console.error("There was an error fetching the data!", error);
        });
    },
    addRow() {
      this.model = {
        id: null,
        name: null,
        description: null,
        price: null
      };
      this.title = 'Adición de Producto';
      this.$refs.dlg.open();
    },
    editRow() {
      if (this.selection) {
        this.editingRow = this.selection;
        this.model = { ...this.selection };
        this.title = 'Modificar Producto';
        this.$refs.dlg.open();
      } else {
        this.$messager.alert({
          title: "Advertencia",
          icon: "warning",
          msg: "Elija una fila para poder modificar"
        });
      }
    },
    saveRow() {
      this.$refs.form.validate(errors => {
        if (!errors) {
          const newRow = { ...this.model };

          if (this.editingRow) {
            // Update existing row
            axios.put(`http://192.168.1.10/aprod/`, newRow)
              .then(response => {
                const index = this.data.findIndex(product => product.id === this.editingRow.id);
                if (index !== -1) {
                  this.data[index] = response.data;
                }
                this.editingRow = null;
                this.fetchData();
                this.$refs.dlg.close();
                this.fetchData();
              })
              .catch(error => {
                console.error("Hubo un error al tratar de actualizar el registro del Producto!", error);
              });
          } else {
            // Create new row
            axios.post('http://192.168.1.10/aprod/index.php', newRow)
              .then(response => {
                this.data.unshift(response.data);
                this.fetchData();
                this.$refs.dlg.close();
                this.fetchData();
              })
              .catch(error => {
                console.error("Hubo un error al tratar de crear el registro del Producto!", error);
              });
          }
          //alert("in")
          this.fetchData();
        }
      });
    },
    deleteRow() {
      if (this.selection) {
        this.$messager.confirm({
          title: 'Confirmar',
          msg: '¿Está seguro de eliminar el registro de este producto?',
          result: (r) => {
            if (r) {
              axios.delete(`http://192.168.1.10/aprod/index.php`, { data: { id: this.selection.id } })
                .then(response => {
                  console.log(response)                
                  this.fetchData();
                })
                .catch(error => {
                  console.error("Hubo un error al tratar de eliminar el registro del producto!", error);
                });
            }
          }
        });
      } else {
        this.$messager.alert({
          title: "Advertencia",
          icon: "warning",
          msg: "Elija una fila para poder eliminar"
        });
      }
    },
    getError(name) {
      return this.errors[name] && this.errors[name].length
        ? this.errors[name][0]
        : null;
    },
    fn_buscar() {
      this.filterable = !this.filterable;
    },
    fn_limpiar() {
      alert("no implementado");
    },
    fn_principal() {
      window.location.href = "https://www.google.com";
    }
  }
};
</script>

<style>
.icon-delete {
  background: url('delete1.png') no-repeat center center;
}
.icon-home {
  background: url('home.png') no-repeat center center;
}
.icon-refresh {
  background: url('refresh.png') no-repeat center center;
}
</style>
