<template>
  <div class="container">
    <AppSidebar :links="links">
      <loading :active="loading" :message="loading_msg" />

      <MaterialSettings
        :slot="0"
        :material="material"
        :course="course"
        :signInFirebase="signInFirebase"
      />
      <CategoriesEditor :slot="1" :material="material" />
      <QuizzesEditor :slot="2" :material="material" />
      <QuestionsEditor :slot="3" :material="material" />
    </AppSidebar>
  </div>
</template>

<script>
import loading from "@/components/loading";
import AppSidebar from "@/components/AppSidebar";
import MaterialSettings from "@/components/MaterialEditor/MaterialSettings";
import CategoriesEditor from "@/components/MaterialEditor/CategoriesEditor";
import QuizzesEditor from "@/components/MaterialEditor/QuizzesEditor";
import QuestionsEditor from "@/components/Questions/QuestionsEditor";

import { getParam } from "@/services/router.js";
import { getMaterial } from "@/services/materialService";
import { getCourseByMaterial } from "@/services/courseService";
import { getCourseToken } from "@/services/firebaseStorageService";

import firebase from "firebase/app";
import "firebase/auth";

export default {
  data: () => ({
    links: [
      {
        image: require("@/assets/braintutor/icon-settings.png"),
        text: "Configuración"
      },
      {
        image: require("@/assets/braintutor/icon-material.png"),
        text: "Material"
      },
      {
        image: require("@/assets/braintutor/icon-quiz.png"),
        text: "Pruebas"
      },
      {
        image: require("@/assets/braintutor/icon-material.png"),
        text: "Preguntas evaluación"
      }
    ],
    material: {},
    course: {},
    //
    loading: false,
    loading_msg: ""
  }),
  async created() {
    this.loading = true;
    this.loading_msg = "Cargando Material";

    let material_id = getParam("material_id");
    try {
      this.material = await getMaterial(material_id);
      this.course = await getCourseByMaterial(material_id);
    } catch (error) {
      this.$root.$children[0].showMessage("Error", error.msg);
    }

    this.loading = false;
  },
  methods: {
    async signInFirebase() {
      let { token } = await getCourseToken(this.course._id.$oid);
      await firebase.auth().signInWithCustomToken(token);
    }
  },
  components: {
    loading,
    AppSidebar,
    MaterialSettings,
    CategoriesEditor,
    QuizzesEditor,
    QuestionsEditor
  }
};
</script>

<style>
</style>