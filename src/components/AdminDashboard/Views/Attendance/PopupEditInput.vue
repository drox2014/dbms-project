<template>
  <div>
    <transition name="modal">
      <div class="modal-mask" @click="close" v-show="showEditModal">
        <div class="modal-container" @click.stop>
          <div class="modal-header">
            <h3>Edit Record</h3>
          </div>
          <div class="modal-body">
            <label class="form-label">
              Absent / Present
              <select v-model="dataObject.absent_present" class="form-control">
                <option value ="0">Absent</option>
                <option value ="1">Present</option>
              </select>
            </label>
          </div>
          <div class="modal-footer text-right">
            <button class="modal-default-button" @click="saveRecord()">
              Update record
            </button>
            <button class="modal-default-button" @click="close()">
              Cancel
            </button>
          </div>
        </div>
      </div>
    </transition>
    <transition name="modal">
      <div class="modal-mask" @click="close" v-show="showDeleteModal">
        <div class="modal-container" @click.stop>
          <div class="modal-header">
            <h3>Delete Record</h3>
          </div>
          <div class="modal-body">
            <label class="form-label">
              Confirm record deletion?
            </label>
          </div>
          <div class="modal-footer text-right">
            <button class="modal-default-button" @click="deleteRecord()">
              Yes
            </button>
            <button class="modal-default-button" @click="close()">
              Cancel
            </button>
          </div>
        </div>
      </div>
    </transition>
    <div>
      <button id="show-modal" @click="showEditModal = true">Edit</button>
      <button id="show-delete-modal" @click="showDeleteModal = true">Delete</button>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      editData: Object
    },
    data () {
      return {
        showEditModal: false,
        showDeleteModal: false,
        dataObject: {},
        title: '',
        body: ''
      }
    },
    methods: {
      saveRecord: function () {
        this.$http.patch('http://localhost:3000/updateAttendance', this.dataObject).then(function (res) {
          if (res.ok && res.status === 200) {
            return alert('Attendance record updated successfully ' + this.dataObject.absent_present)
          }
          alert('Unable to update this attendance record 1')
        }).catch(function (err) {
          console.log(err)
          alert('Unable to update this attendance record 2')
        })
        this.close()
      },
      deleteRecord: function () {
        this.$http.post('http://localhost:3000/removeAttendance', this.dataObject).then(function (res) {
          if (res.ok && res.status === 200) {
            return alert('Attendance record deleted successfully')
          }
          alert('Unable to delete attendance record-' + this.dataObject.studentId + this.dataObject.classId + this.dataObject.attendDate)
        }).catch(function (err) {
          console.log(err)
          alert('Unable to delete attendance record--' + this.dataObject.studentId)
        })
        this.close()
      },
      close: function () {
        this.showEditModal = false
        this.showDeleteModal = false
        this.title = ''
        this.body = ''
      }
    },
    mounted: function () {
      this.dataObject.studentId = this.editData.student_id
      this.dataObject.classId = this.editData.class_id
      this.dataObject.attendDate = this.editData.attend_date
      this.dataObject.absent_present = this.editData.is_present
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
