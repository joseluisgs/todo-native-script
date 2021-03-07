<template>
  <Page class="page">
    <!-- Barra de título -->
    <ActionBar title="Mis Tareas App" class="action-bar" />
    <!-- Mi pantalla tendrá un TabView -->
    <TabView
      height="100%"
      androidTabsPosition="bottom"
      selectedTabTextColor="#53ba82"
      tabTextFontSize="15"
    >
      <!--  Tabview de Activas -->
      <TabViewItem title="📑 Activas" textTransform="uppercase">
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
            separatorColor="transparent"
          >
            <v-template>
              <Label
                id="active-task"
                :text="todo.name"
                class="list-group-item-heading"
                textWrap="true"
              />
            </v-template>
          </ListView>
        </StackLayout>
      </TabViewItem>

      <!-- Tabview de Completadas -->
      <TabViewItem title="🗃️ Completadas" textTransform="uppercase">
        <ListView
          class="list-group"
          for="done in dones"
          @itemTap="onDoneTap"
          style="height: 75%"
          separatorColor="transparent"
        >
          <v-template>
            <Label
              id="completed-task"
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

TextField {
  font-size: 20;
  color: #53ba82;
  margin-top: 10;
  margin-bottom: 10;
  margin-right: 5;
  margin-left: 20;
}

Button {
  font-size: 20;
  font-weight: bold;
  color: white;
  background-color: #53ba82;
  height: 40;
  margin-top: 10;
  margin-bottom: 10;
  margin-right: 10;
  margin-left: 10;
  border-radius: 20px;
}

#active-task {
  font-size: 20;
  font-weight: bold;
  color: #53ba82;
  margin-left: 20;
  padding-top: 5;
  padding-bottom: 10;
}

#completed-task {
  font-size: 20;
  color: #d3d3d3;
  margin-left: 20;
  padding-top: 5;
  padding-bottom: 10;
  text-decoration: line-through;
}

</style>
