# CRUD Aplicacion sobre Producto con rest api y VueJS-EasyUI -COVEPRA
## Operaciones: Adicion, modificacion, eliminacion y busqueda
## https://github.com/fabinnerself/covep.git
### Descripcion

Este proyecto implementa operaciones CRUD completas (Crear, Leer, Actualizar y Eliminar), junto con filtrado y paginación en un grid a datos rest api. A continuación, se describen las funcionalidades principales:

*Operaciones CRUD y Filtrado: Todas las operaciones de CRUD y filtrado son completamente funcionales.

*Grid con Filtros: El grid permite filtrar datos por todos los campos.

*Ajuste de Columnas: Es posible cambiar el ancho de las columnas en el grid.

*Paginación: El grid incluye paginación y permite seleccionar el número de elementos por página .

*Función de Limpiar Filtros: Actualmente, la función para limpiar campos de filtro no está operativa.

*Función formato numerico: Actualmente, el formato numerico a las columnas del grid tipo numerico está operativa.

### Requisitos del Sistema

- **Sistema Operativo**: Ubuntu 22.04.4 LTS
- **Kernel**: Version 6.2.16-3-pve
- **EasyUI**: Version 1.10.19  / v3-easyui: ^3.0.14 (package.json)
- **NodeJS**: Version v20.15.1
- **nvm**: Version 0.39.7
- **npm/npx**: 10.7.0
- **vuejs cli**: @vue/cli 5.0.8
- **vuejs package.json**: ^3.2.13


Sistemas Alternativos Probados
El proyecto también ha sido probado en el siguiente entorno alternativo:

- **Sistema Operativo**: Windows 10 Pro 1803
- **EasyUI**: Version 1.10.19  / v3-easyui: ^3.0.14 (package.json)
- **NodeJS**: Version v20.15.1
- **npm/npx**: 10.7.0
- **vuejs cli**: @vue/cli 5.0.8
- **vuejs package.json**: ^3.2.13

## notas de instalacion
1. Crear la App Vue 
	vue create my-vue-app
	cd my-vue-app
    npm install (opcional)
	npm run serve    
2.  Instalar  EasyUI para Vuejs 3.
    npm install v3-easyui --save
	npm install axios

3. Importar  EasyUI en /src/main.js
import { createApp } from 'vue';
import App from './App.vue';
import 'v3-easyui/dist/themes/default/easyui.css';
import 'v3-easyui/dist/themes/icon.css';
import 'v3-easyui/dist/themes/vue.css';
import EasyUI from 'v3-easyui';

4. Importar los components a '/src/App.vue'.
<template>
    <div>
        <DataGrid :data="data" style="height:250px">
            <GridColumn field="itemid" title="Item ID"></GridColumn>
            <GridColumn field="name" title="Name"></GridColumn>
            <GridColumn field="listprice" title="List Price" align="right"></GridColumn>
            <GridColumn field="unitcost" title="Unit Cost" align="right"></GridColumn>
            <GridColumn field="attr" title="Attribute" width="30%"></GridColumn>
            <GridColumn field="status" title="Status" align="center"></GridColumn>
        </DataGrid>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                data: [
                    {"code":"FI-SW-01","name":"Koi","unitcost":10.00,"status":"P","listprice":36.50,"attr":"Large","itemid":"EST-1"},
                    {"code":"K9-DL-01","name":"Dalmation","unitcost":12.00,"status":"P","listprice":18.50,"attr":"Spotted Adult Female","itemid":"EST-10"},
                    {"code":"RP-SN-01","name":"Rattlesnake","unitcost":12.00,"status":"P","listprice":38.50,"attr":"Venomless","itemid":"EST-11"},
                    {"code":"RP-SN-01","name":"Rattlesnake","unitcost":12.00,"status":"P","listprice":26.50,"attr":"Rattleless","itemid":"EST-12"},
                    {"code":"RP-LI-02","name":"Iguana","unitcost":12.00,"status":"P","listprice":35.50,"attr":"Green Adult","itemid":"EST-13"},
                    {"code":"FL-DSH-01","name":"Manx","unitcost":12.00,"status":"P","listprice":158.50,"attr":"Tailless","itemid":"EST-14"},
                    {"code":"FL-DSH-01","name":"Manx","unitcost":12.00,"status":"P","listprice":83.50,"attr":"With tail","itemid":"EST-15"},
                    {"code":"FL-DLH-02","name":"Persian","unitcost":12.00,"status":"P","listprice":23.50,"attr":"Adult Female","itemid":"EST-16"},
                    {"code":"FL-DLH-02","name":"Persian","unitcost":12.00,"status":"P","listprice":89.50,"attr":"Adult Male","itemid":"EST-17"},
                    {"code":"AV-CB-01","name":"Amazon Parrot","unitcost":92.00,"status":"P","listprice":63.50,"attr":"Adult Male","itemid":"EST-18"}
                ]
            }
        }
    }
</script>

Extraido y adecuado de https://www.jeasyui.com/download/vx12.php

## Comandos utiles
lsb_release -a && uname -r

node -v && nvm -v && npm -v && npx -v && vue -V 

(C) 2024 Favian Medina Gemio
