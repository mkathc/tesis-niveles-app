<template>
  <div class="container pa-0">
    <div class="history">
      <span class="history__back" @click="redirect()">Cursos</span>
      <span class="history__divider">></span>
      <span v-if="course">{{course.name}}</span>
      <span v-else>...</span>
    </div>
    <AppSidebar :links="links">
      <PreEvaluation :slot="0" />
      <Chatbots :slot="1" />
      <Tasks :slot="2" />
      <Events :slot="3" />
      <Evaluations :slot="4" />
      <Students :slot="5" :get="getStudents" />
    </AppSidebar>
  </div>
</template>

<script>
import AppSidebar from "@/components/AppSidebar";
import PreEvaluation from "@/components/Session/PreEvaluations";
import Chatbots from "@/components/Session/Chatbots";
import Tasks from "@/components/Session/Tasks";
import Events from "@/components/Session/Events/index";
import Evaluations from "@/components/Session/Evaluations/index";
import Students from "@/components/Students/index";

import { redirect, getParam } from "@/services/router";
import { getCourseNameBySession } from "@/services/courseService";
import { getStudentsBySessionStudent } from "@/services/studentService";

export default {
  data: () => ({
    course: null,
    session_id: "",
    links: [
      {
        image:
          "https://www.flaticon.com/premium-icon/icons/svg/207/207190.svg",
        text: "Pre Evaluación"
      },
      {
        image:
          "https://images.squarespace-cdn.com/content/v1/55d1e076e4b0be96a30dc726/1477412415649-WW90BD77ALIB9U99VTIA/ke17ZwdGBToddI8pDm48kDmvgM2_GYudIur22MWWiLdZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZamWLI2zvYWH8K3-s_4yszcp2ryTI0HqTOaaUohrI8PIvFa2r33EMaMk7hlBJBei4G1FTiqzsF6lpp3EXtW1YCk/image-asset.png",
        text: "Aprender"
      },
      {
        image:
          "https://limpiasol.com.ar/sitio/wp-content/uploads/2016/09/task-done-flat.png",
        text: "Tareas"
      },
      {
        image: require("@/assets/braintutor/icon-event.png"),
        text: "Agenda"
      },
      {
        image: require("@/assets/braintutor/icon-exam.png"),
        text: "Evaluaciones"
      },
      {
        image: require("@/assets/braintutor/icon-students.png"),
        text: "Alumnos"
      }
    ]
  }),
  async mounted() {
    this.session_id = getParam("session_id");
    this.course = await getCourseNameBySession(this.session_id);
  },
  methods: {
    redirect() {
      redirect("sessions-student");
    },
    async getStudents() {
      return await getStudentsBySessionStudent(this.session_id);
    }
  },
  components: {
    AppSidebar,
    PreEvaluation,
    Tasks,
    Chatbots,
    Events,
    Evaluations,
    Students
  }
};
</script>

<style lang='scss' scoped>
</style>