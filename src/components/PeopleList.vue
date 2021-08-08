<template>
  <div>
    <div>
        <span style="float:left;"><h3>Sorting Training System</h3></span>
        <span style="float:right;"><button type="button" class="btn btn-warning" data-toggle="modal" data-target="#popup">Start sorting!</button></span>
    </div><br><br>

      <table class="table table-bordered">
        <thead>
        <tr>
        <th scope="col">Email</th>
        <th scope="col">Potatoes</th>
        <th scope="col">Tags</th>
        <th scope="col">Full Name</th>
        <th scope="col">Location</th>
        </tr>
        </thead>
        <draggable v-model="list" tag="tbody" @end="Sorting">
          <tr v-for="item in list" :key="item.id">
            <td>
                <label class="orange-checkbox-container">{{ item.email }}
                <input type="checkbox">
                <span class="checkmark"></span>
            </label>
            </td>
            <td>{{ item.potatoes }}</td>
            <td class="badge badge-light">{{ item.tags }}</td>
            <td>{{ item.fullName }}</td>
            <td>{{ item.location }}</td>
          </tr>
        </draggable>
      </table>


<div class="modal" id="popup" tabindex="-1" role="dialog">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <p>Enter a number of how many people you want to add to the list.</p>
        <div class="input-group input-group-sm mb-3">
        <input type="number" v-model = "count" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
        </div>
      </div>
      
      <div class="modal-footer">
        <button type="button" @click="Cancel" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
        <button type="button" @click="PopulateTable" class="btn btn-warning">Start</button>
      </div>
    </div>
  </div>
</div>

  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: "table-example",
  display: "Table",
  order: 8,
  components: {
    draggable
  },
  data() {
    return {
      list: [],
      dragging: false,
      count: 20,
      startTime: 0,
      endTime: 0,
      sortedPotatoeList: []
    };
  },
  methods: {
      PopulateTable(){
        if(this.count < 20 || this.count > 100){
          alert("Number should be between 20 to 100");
          return;
        }
         let firstNames = ["Ally", "Alex", "Lyla", "Kylia", "Bryce", "Daniela", "Jacob", "Reily", "Micaela", "Breonia","Jessica"];
         let lastNames = ["Aagaard", "Abadi", "Abbatiello", "Abbot", "Abella", "Abraham", "Acevedo", "Adam", "Adams","Adcock", "Agan"];

         this.list = [];
         this.sortedPotatoeList = [];
         let emails = [];
         let names = [];
         let potatoeList = [];

         for(let i = 0; i<this.count; i++){
              let potato = Math.floor(Math.random() * 1000);
              if(potatoeList.includes(potato))
                    i--;
              else{
                potatoeList.push(potato);
                this.sortedPotatoeList.push(potato); // create separate list for sorting
              }
         }

         for(let i = 0; i<this.count; i++){
             let pickFirst = Math.floor(Math.random() * 10);
             let pickLast = Math.floor(Math.random() * 10);
             let name = firstNames[pickFirst]+" "+lastNames[pickLast];
             let email = firstNames[pickFirst]+lastNames[pickLast]+"@gmail.com";
             emails.push(email.toLowerCase());
             names.push(name);
         }
        //  console.log(potatoeList);
        //  console.log(emails);
        //  console.log(names);
         this.sortedPotatoeList.sort(function(a, b){return b - a});
        //  console.log(this.sortedPotatoeList);
         this.list = [];
         for(let i = 0; i<this.count; i++){
             let element = {
                 id: i+1,
                 email: emails[i],
                 potatoes: potatoeList[i],
                 tags: "Customers",
                 fullName: names[i],
                 location: "Lithuania"
             }

             this.list.push(element)
             this.start()
            //  let modal = document.getElementById('popup');
            //  modal.style.display = 'none';
         }
      },
      start() {
        this.startTime = new Date();
       },

      end() {
        this.endTime = new Date();
        let timeDiff = this.endTime - this.startTime; //in ms
        // strip the ms
        timeDiff /= 1000;

        // get seconds 
        let seconds = Math.round(timeDiff);
        // console.log(seconds + " seconds");
        return seconds;
      },
      Cancel(){
          this.list = [];
          this.startTime = 0;
          this.endTime = 0;
      },
      Sorting(){
          let tempPotatoes = [];
          let sortedPotatoes = []
          for(let i=0; i<this.count; i++){
            tempPotatoes.push(this.list[i].potatoes)
          }
          sortedPotatoes = JSON.stringify(this.sortedPotatoeList);
          tempPotatoes = JSON.stringify(tempPotatoes);

          // console.log("Temp Potatoes",tempPotatoes);
          // console.log("Sorted Potatoes",sortedPotatoes);

          if(sortedPotatoes == tempPotatoes){
            let score = this.end();
            alert("Score : "+ score);
          }
          else{
            // console.log("Not Sorted");
          }

          
      },
  }
};
</script>
<style scoped>
    .buttons {
      margin-top: 35px;
    }

    tr:hover {background-color:#f5f5f5;}

    /* Customize the label (the container) */
    .orange-checkbox-container {
      display: block;
      position: relative;
      cursor: pointer;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
    }

    /* Hide the browser's default checkbox */
    .orange-checkbox-container input {
      position: absolute;
      opacity: 0;
      cursor: pointer;
    }

    /* Create a custom checkbox */
    .checkmark {
      position: absolute;
      top: 0;
      left: 0;
      height: 15px;
      width: 15px;
      background-color: #bec5d1;
    }

    /* When the checkbox is checked, add a orange background */
    .orange-checkbox-container input:checked ~ .checkmark {
      background-color: #FFB000;
    }

    /* Create the checkmark/indicator (hidden when not checked) */
    .checkmark:after {
      content: "";
      position: absolute;
      display: none;
    }

    /* Show the checkmark when checked */
    .orange-checkbox-container input:checked ~ .checkmark:after {
      display: block;
    }

    /* Style the checkmark/indicator */
    .orange-checkbox-container .checkmark:after {
      left: 5px;
      top: 2px;
      width: 5px;
      height: 10px;
      border: solid #001014;
      border-width: 0 3px 3px 0;
      -webkit-transform: rotate(45deg);
      -ms-transform: rotate(45deg);
      transform: rotate(45deg);
    }
</style>