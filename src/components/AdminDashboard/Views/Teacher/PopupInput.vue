<template>
  <div>
    <transition name="modal">
        <div class="modal-mask" @click="close" v-show="showModal">
            <div class="modal-container" @click.stop>
                <div class="modal-header">
                    <h3>New Teacher</h3>
                </div>
                <div class="modal-body">
                    <label class="form-label">
                      First Name
                      <input v-model="dataObject.firstName" class="form-control" name="fName" v-validate="'required|alpha'">
                    </label>
                    <span v-show="errors.has('fName')" style="color:red">Invalid name</span>
                    <label class="form-label">
                      Last Name
                      <input v-model="dataObject.lastName" class="form-control" name="teacherName" v-validate="'required|alpha'">
                    </label>
                    <span v-show="errors.has('teacherName')" style="color:red">Invalid name</span>
                    <label class="form-label">
                      Address Line 1
                      <input v-model="dataObject.address1" class="form-control" name="add1" v-validate="'required'">
                    </label>
                    <span v-show="errors.has('add1')" style="color:red">Invalid address</span>
                    <label class="form-label">
                      Address Line 2
                      <input v-model="dataObject.address2" class="form-control" name="add2" v-validate="'required'">
                    </label>
                    <span v-show="errors.has('add2')" style="color:red">Invalid address</span>
                    <label class="form-label">
                      Address Line 3
                      <input v-model="dataObject.address3" class="form-control" name="add3" v-validate="'required'">
                    </label>
                    <span v-show="errors.has('add3')" style="color:red">Invalid address</span>
                </div>
                <div class="modal-footer text-right">
                    <button class="modal-default-button" @click="saveRecord()">
                        Add new teacher
                    </button>
                    <button class="modal-default-button" @click="close()">
                        Cancel
                    </button>
                </div>
            </div>
        </div>
    </transition>
    <div>
        <button id="show-modal" @click="showModal = true">Add Record</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {

  },
  data () {
    return {
      dataObject: {},
      title: '',
      body: '',
      showModal: false
    }
  },
  methods: {
    open: function () {

    },
    saveRecord: function () {
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.$http.post('http://localhost:3000/addNewTeacher', this.dataObject).then(function (res) {
            if (res.ok && res.status === 200) {
              return alert('Teacher added successfully')
            }
            alert('Unable to register this teacher')
          }).catch(function (err) {
            console.log(err)
            alert('Unable to register this teacher')
          })
          this.close()
        }
      })
    },
    close: function () {
      for (var key in this.dataObject) {
        delete this.dataObject[key]
      }
      this.showModal = false
      this.title = ''
      this.body = ''
    }
  },
  mounted: function () {
    document.addEventListener('keydown', (e) => {
      if (e.keyCode === 27) {
        this.close()
      }
    })
  }
}
</script>

<style scoped>
* {
    box-sizing: border-box;
}

.modal-mask {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .5);
    transition: opacity .3s ease;
}

.modal-container {
    width: 300px;
    margin: 40px auto 0;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
    transition: all .3s ease;
    font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
    margin-top: 0;
    color: #42b983;
}

.modal-body {
    margin: 20px 0;
}

.text-right {
    text-align: right;
}

.form-label {
    display: block;
    margin-bottom: 1em;
}

.form-label > .form-control {
    margin-top: 0.5em;
}

.form-control {
    display: block;
    width: 100%;
    padding: 0.5em 1em;
    line-height: 1.5;
    border: 1px solid #ddd;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>
