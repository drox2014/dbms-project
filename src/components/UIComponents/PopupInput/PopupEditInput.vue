<template>
  <div>
    <transition name="modal">
        <div class="modal-mask" @click="close" v-show="showEditModal">
            <div class="modal-container" @click.stop>
                <div class="modal-header">
                    <h3>Edit Record</h3>
                </div>
                <div class="modal-body" v-for="(attr,index) in attributes">
                    <label class="form-label" v-if="attr!='Options'">
                        {{attr}}
                        <input :id="attr + 'Edit'" class="form-control" :value="editData[dbRows[index]]" readonly="true" v-if="index==0">
                        <input :id="attr + 'Edit'" class="form-control" :placeholder="editData[dbRows[index]]" v-if="index!=0">
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
    <div>
        <button id="show-modal" @click="showEditModal = true">Edit</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    attributes: Array,
    dbRows: Array,
    databaseTable: '',
    editData: Object
  },
  data () {
    return {
      showEditModal: false,
      dataObject: {},
      title: '',
      body: ''
    }
  },
  methods: {
    saveRecord: function () {
      this.dataObject.databaseTable = this.databaseTable
      for (var i in this.attributes) {
        var key = this.attributes[i] + 'Edit'
        var value = document.getElementById(key).value
        var placeholder = document.getElementById(key).placeholder
        if (value !== '') {
          // alert(value)
        } else {
          // alert(placeholder)
        }
      }
      this.showEditModal = false
    },
    close: function () {
      this.showEditModal = false
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
