<template>
  <div class="body">
    <h1>Alunos</h1>
    <button class="add" v-if="buttonAdd" @click="formVisibility()">
      ADICIONAR
    </button>
    <form v-if="inputForm">
      <div class="form">
        <input type="text" placeholder="Nome" v-model="studentInput.name" />
        <br />
        <input type="text" placeholder="Idade" v-model="studentInput.age" />
        <div class="form-buttons">
          <button class="buttonCancel" @click="addButtonVisibility()">
            CANCELAR
          </button>
          <button class="buttonSave" @click.prevent="addNewStudent()">
            SALVAR
          </button>
        </div>
      </div>
    </form>
    <form v-if="editForm">
      <div class="form">
        <input type="text" placeholder="Nome" v-model="editStudentInput.name" />
        <br />
        <input type="text" placeholder="Idade" v-model="editStudentInput.age" />
        <div class="form-buttons">
          <button class="buttonCancel" @click="addButtonVisibility()">
            CANCELAR
          </button>
          <button class="buttonSave" @click.prevent="saveEditStudent(editStudentInput.id,editStudentInput.name,editStudentInput.age)">
            SALVAR EDIÇÃO
          </button>
        </div>
      </div>
    </form>
    <div v-for="element in students" class="students">
      <h2>Nome: {{ element.name }}</h2>
      <p>Idade: {{ element.age }}</p>
      <div class="form-buttons">
        <button @click="editStudent(element.id,`${element.name}`,element.age)">EDITAR</button>
        <button class="buttonRemoveStudent" @click="removeStudent(element.id)">APAGAR</button>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttonAdd: true,
      inputForm: false,
      editForm: false,
      studentInput: {
        id: "",
        name: "",
        age: "",
      },
      editStudentInput: {
        id: "",
        name: "",
        age: "",

      },  
      students: [],
    };
  },
  computed: {
    //Propiedades computadas
  },
  mounted() {
    // Carregamento de funções após carregamento do dom e propriedades data()
  },
  methods: {
    formVisibility() {
      this.buttonAdd = false;
      this.inputForm = true;
    },
    addButtonVisibility() {
      this.inputForm = false;
      this.buttonAdd = true;
    },
    addNewStudent() {
      if (this.studentInput.name === "" || this.studentInput.age === "") {
        window.alert("Preencha todos os campos antes de salvar");
      } else {
        this.students.push({
          id: this.students.length,
          name: this.studentInput.name,
          age: this.studentInput.age,
        })
      
        this.studentInput.name = ''
        this.studentInput.age = ''      
      }
    },
    removeStudent(index) {
      const studentIndex = this.students.findIndex(element => element.id === index);
      this.students.splice(studentIndex, 1);
    },
    editStudent(id,name,age) {
      this.inputForm = false
      this.editForm = true 
      this.editStudentInput.id = id
      this.editStudentInput.name = name
      this.editStudentInput.age = age
      console.log(id,name,age)
    },
    saveEditStudent(id,name,age){
      this.students.forEach(e => {
        if(e.id === id) {
          e.name = name
          e.age = age
        }

        this.editForm= false
        this.inputForm = true
      })
    }
  },
};
</script>

<style scoped>
.body {
  margin: 0 50vw;
}

.add {
  background-color: green;
  color: white;
  padding: 20px;
}

.form {
  display: grid;
  grid-template-rows: repeat(5, 40px);
}

.form-buttons {
  display: flex;
  flex-direction: row;
  gap: 35px;
  grid-row: 5/6;
}

.buttonCancel,
.buttonSave {
  padding: 4px;
}

.buttonCancel {
  background-color: gray;
  color: white;
}

.students {
  background-color: gray;
  box-sizing: border-box;
}

.buttonSave {
  background-color: green;
  color: white;
}

.buttonRemoveStudent {
  background-color: red;
  color: white;
}
</style>
