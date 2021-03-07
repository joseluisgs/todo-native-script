<template>
  <Page class="page">
    <!-- Barra de título -->
    <ActionBar title="Mis Tareas App" class="action-bar" />
    <!-- Mi pantalla tendrá un TabView -->
    <TabView height="100%" androidTabsPosition="bottom">
      <!--  Tabview de Activas -->
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

      <!-- Tabview de Completadas -->
      <TabViewItem title="🗃️ Completadas">
        <ListView
          class="list-group"
          for="done in dones"
          @itemTap="onDoneTap"
          style="height: 75%"
        >
          <v-template>
            <Label
              :text="done.name"
              class="list-group-item-heading"
              textWrap="true"
            />
          </v-template>
        </ListView>
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
    nuevaTarea: "",
    dones: []
  }),

  // Mis metodos
  methods: {
    // Evento de pulsar Item activo, usamos el function por el this
    onItemTap(args) {
      action("¿Que quieres hacer?", "Cancelar", [
        "Archivar tarea",
        "Borrar tarea"
      ]).then(result => {
        console.log(result);
        switch (result) {
          case "Archivar tarea":
            // Quitamos de la lista de activa y la encolamos en la de terminadas
            this.dones.unshift(args.item);
            this.todos.splice(args.index, 1);
            break;
          case "Borrar tarea":
            // Borramos de la lista
            this.todos.splice(args.index, 1);
            break;
          case "Cancelar" || undefined: // Cerramos
            break;
        }
      });
    },

    // Evento para eliminar activas
    onDoneTap(args) {
      action("¿Que quieres hacer?", "Cancelar", [
        "Activar tarea",
        "Borrar tarea"
      ]).then(result => {
        console.log(result);
        switch (result) {
          case "Activar tarea":
            // Quitamos de lista de completadas y ponemos en pendientes
            this.todos.unshift(args.item);
            this.dones.splice(args.index, 1);
            break;
          case "Borrar tarea":
            // Eliminamos de la lista
            this.dones.splice(args.index, 1); // Removes the tapped completed task.
            break;
          case "Cancelar" || undefined: // Cerramos
            break;
        }
      });
    },

    // Evento de botón
    onButtonTap() {
      if (this.nuevaTarea === "") return;
      console.log(`Nueva tarea añadida: ${this.nuevaTarea}.`);
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
