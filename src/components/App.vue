<template>
  <Page class="page">
    <!-- Barra de título -->
    <ActionBar title="Mis Tareas App" class="action-bar" />
    <!-- Mi pantalla tendrá un TabView -->
    <TabView height="100%" androidTabsPosition="bottom">
      <TabViewItem title="📑 Activas">
        <!-- Usamos un layaou del tipo Stack horizontal, componentes apilados -->
        <StackLayout orientation="vertical" width="100%" height="100%">
          <!-- Lo dividmos en dos secciones, la primera es un Grid de dos columnas -->
          <GridLayout columns="2*,*" rows="*" width="100%" height="25%">
            <!-- Cuadro de texto enlzado al modelo de datos, si pulsas intro o botón hacemos lo mismo. -->
            <TextField
              col="0"
              row="0"
              v-model="nuevaTarea"
              hint="Nueva Tarea..."
              editable="true"
              @returnPress="onButtonTap"
            />

            <Button col="1" row="0" text="Añadir" @tap="onButtonTap" />
          </GridLayout>

          <!-- Lista en un ListView -->
          <ListView
            class="list-group"
            for="todo in todos"
            @itemTap="onItemTap"
            style="height: 75%"
          >
            <v-template>
              <Label
                :text="todo.name"
                class="list-group-item-heading"
                textWrap="true"
              />
            </v-template>
          </ListView>
        </StackLayout>
      </TabViewItem>
      <TabViewItem title="🗃️ Completadas">
        <Label
          text="This tab will list completed tasks for tracking."
          textWrap="true"
        />
      </TabViewItem>
    </TabView>
  </Page>
</template>

<script>
export default {
  // mi nombre
  name: "App",

  // Mi modelo de datos
  data: () => ({
    todos: [],
    nuevaTarea: ""
  }),

  // Mis metodos
  methods: {
    // Evento de pulsar Item
    onItemTap: (args) => {
      console.log("Item con índice: " + args.index + " pulsado");
    },

    // Evento de botón
    onButtonTap() {
      if (this.nuevaTarea === '') return;
      console.log("Nueva tarea añadida: " + this.nuevaTarea + "."); 
      this.todos.unshift({ name: this.nuevaTarea }); 
      this.nuevaTarea = ""; 
    }
  }
};
</script>

<style scoped>
ActionBar {
  background-color: #53ba82;
  color: #ffffff;
}

.message {
  vertical-align: center;
  text-align: center;
  font-size: 20;
  color: #333333;
}
</style>
