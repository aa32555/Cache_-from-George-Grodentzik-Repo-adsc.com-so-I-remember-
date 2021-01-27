<template>
  <div>
    <!-- <p>Form Options are: {{ data.FormOptions[0] }}</p>
    <p>InsOptions are: {{ data.InsOptions[0] }}</p> -->
    <p>Selected Ins is: {{ data.SelectedIns }}</p>

    <div id="app">
      <el-row :gutter="20">
        <el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="8"
          ><div class="grid-content bg-purple">
            This is the appeals page
          </div></el-col
        >
        <el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
      </el-row>

      <el-row :gutter="20">
        <el-col :span="8"><div class="grid-content"></div></el-col>
        <el-col :span="4"
          ><div class="grid-content bg-purple">
            <el-input
              v-model="data.claim"
              placeholder="enter ICN number"
            ></el-input></div
        ></el-col>
       <el-col :span="5"
          ><div class="grid-content bg-purple">
            <el-button :disabled = "selections.claim != ''" type="primary" round v-on:click="getInsInfo()"
              >Submit ICN</el-button
            >
          </div></el-col>

        <el-col :span="3"><div class="grid-content bg-purple"></div></el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="4"
          ><div class="grid-content bg-purple">
            <!-- <select v-model="insSelected">
              <option v-for="ins in data.InsOptions" :value="ins" :key="ins">
                {{ ins.split("#")[1] }}
              </option>
            </select> -->

            <el-select v-model="selections.SI" placeholder="Insurance">
              <el-option
                v-for="ins in data.InsOptions"
                :key="ins"
                :label="stringSplit(ins, 1)"
                :value="stringSplit(ins, 0)"
              >
              </el-option>
            </el-select></div
        ></el-col>
        <el-col :span="4"
          ><div class="grid-content bg-purple">
            <el-select v-model="selections.SF" placeholder="Form">
              <el-option
                v-for="form in data.FormOptions"
                :key="form"
                :label="stringSplit(form, 1)"
                :value="stringSplit(form, 0)"
              >
              </el-option>
            </el-select>
            <!-- <el-input v-model="data.FormOptions" placeholder="Form"></el-input> -->
          </div></el-col
        >
        <el-col :span="4"
          ><div class="grid-content bg-purple">
            <el-date-picker
              v-model="selections.DN"
              type="date"
              placeholder="Pick a day"
              style="vertical-align: top; padding-top: 2px; width: 140px"
              size="mini"
            />
            <!-- <el-input v-model="data.date" placeholder="Date"></el-input> -->
          </div></el-col
        >
        <el-col :span="4"
          ><div class="grid-content bg-purple">
            <el-input
              v-model="selections.prepTitle"
              placeholder="Enter your Title"
            ></el-input></div
        ></el-col>
        <el-col :span="4"
          ><div class="grid-content bg-purple">
            <el-input
              v-model="selections.prepName"
              placeholder="Enter your Name"
            ></el-input></div
        ></el-col>
        <el-col :span="4"
          ><div class="grid-content bg-purple">
            <el-input
              v-model="selections.prepEmail"
              placeholder="Enter your email address"
            ></el-input></div
        ></el-col>
        <!-- <el-col :span="2"><div class="grid-content bg-purple"></div></el-col> -->
      </el-row>

      <el-row :gutter="20">
        <el-col :span="6"
          ><div class="grid-content bg-purple">
            <ul id="displaydata">
              <li v-for="items in selections" :key="items">
                {{ items }}
              </li>
            </ul>
          </div></el-col
        >

        <el-col :span="10"
          ><div class="grid-content bg-purple">
            <ul id="display status">
              <li v-for="items in statusResults" :key="items">
                {{ items }}
              </li>
            </ul>
          </div></el-col
        >

        <el-col :span="6"
          ><div class="grid-content bg-purple">
            <p>Message is: {{ data.claim }}</p>
            <p>Account is: {{ data.acct }}</p>
            <p>Transaction is: {{ data.TR }}</p>
            <!-- <p>Form Options are: {{ data.FormOptions[0] }}</p> -->
            <p>Form Options are:</p>
            <ul id="formoptions">
              <li v-for="form in data.FormOptions" :key="form">
                {{ form }}
              </li>
            </ul>
            <p>Insurance Options are:</p>
            <ul id="insoptions">
              <li v-for="ins in data.InsOptions" :key="ins">
                {{ ins }}
              </li>
            </ul>
          </div></el-col
        >
        <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="16"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="10"
          ><div class="grid-content bg-purple">
            {{ statusResults.data.status }}
            {{ statusResults.data.timePast }}
            {{ statusDisplay }}
          </div></el-col
        >
        <el-col :span="5"
          ><div class="grid-content bg-purple">
           
            >
          </div></el-col
        >
        <el-col :span="5"
          ><div class="grid-content bg-purple">
            <el-button :disabled="selections.DN ==='' || selections.ACCT === ''" type="primary" round v-on:click="createDocuments()"
              >Create Documents</el-button
            >
          </div></el-col
        >
        <el-col :span="4"
          ><div class="grid-content bg-purple">
            <el-button type="primary" round v-on:click="updateStatus()"
              >updateStatus</el-button
            >
                        <!-- <a href="./" download="96bf7e334d614591821a4cbcfbb4aecd.pdf">Download</a> -->
                  <el-button type="primary" round v-on:click="downloadExport()"
              >download file</el-button
            >         
          </div></el-col
        >
      </el-row>
    </div>
  </div>
</template>
<script>
import { M } from "@/api/index";
import axios from 'axios'
export default {
  data() {
    return {
      data: {
        claim: "003432048761",   //"06282016",
        insurance: "",
        form: "",
        acct: "",
        FormOptions: [],
        InsOptions: [],
        SelectedForm: 0,
        SelectedIns: "",
        TR: "",
        tempString: "",
      },
      results: {},
      value: "",
      insArray: [],
      formArray: [],
      selections: {
        prepTitle: "Title",
        prepName: "Name",
        prepEmail: "aaa@adsc.com",
        SI: 1,
        SF: 0,
        DN: "",
        ACCT: "",
        TR: "",
        claim:"",
        
      },
      statusDisplay: "About to begin",
      JOB: "",
      ICN: "",
      ctr: 0,
      statusResults: {
        data: {
          status: "Not Started",
          timePast: 0,
        },
      },
    };
  },
  methods: {
    updateStatus() {
      this.statusDisplay = "Status Call";
    },
    stringSplit(value, no) {
      if (value != "") {
        return value.split("#")[no];
      } else {
        return value;
      }
    },
    async createDocuments() {
      if (this.selections.DN === "")
      {
        alert("Please enter a date");
        return
      }
      debugger;
      if (this.selections.ACCT ===""){
        alert("Please enter the claim number and submit to  retrieve an account")
        return;
      }
      if (this.selections.ACCT != "") {
        var results = [];
         this.selections.claim = this.data.claim;
        alert(this.selections.claim);
        results = await M("getClaimAndEob^GJGTEST", this.selections);
        // Pass results that contains the job number to check for
        this.getStatus(results);
      }
    },
    async getStatus1(results) {
      // Setting upper case JOB becasue lower case is returned.
      debugger
      results.data.JOB = results.data.job;
      this.JOB = results.data.job; 
      debugger;
      this.statusResults = await M("getStatus^GJGTEST", results.data);
    },
    async getFinalStatus(results) {
      // Setting upper case JOB becasue lower case is returned.
      debugger
      results.data.JOB = results.data.job;

      debugger;
      this.statusResults = await M("getFinalStatus^GJGTEST", results.data);
    },
    getStatus(results) {
      var startTime, endTime;
      var seconds;
      startTime = new Date();
      this.ctr = 0;

      var timer = setInterval(
        function () {
          this.ctr = this.ctr + 1;
          console.log(this.ctr);

          ////

          endTime = new Date();
          var timeDiff = endTime - startTime; //in ms
          // strip the ms
          timeDiff /= 1000;
          this.timePast = timeDiff;

          // get seconds
          seconds = Math.round(timeDiff);
          console.log(seconds + " seconds");
          if (this.statusResults.data.status === "DONE" || this.ctr > 10) {
            this.getFinalStatus(results);  // call one more time for last status
            clearInterval(timer);
           
          }
          this.getStatus1(results);
          this.statusDisplay = this.timePast;
          debugger;
        }.bind(this),
        2000
      );
    },
 async getPDFfile(){
    
        //this.results =  await M("PDF^MWGCR", this.results.data);
         return  await M("PDF^TESTG");
    
  },
     // example of downloading file. 
     

      downloadExport() {
        debugger
        axios.get( process.env.BASE_API +  'premier/PDF^TESTG' + "?"  + this.JOB , {
            // If you forget this, your download will be corrupt.
            responseType: 'blob'
        }).then((response) => {
            // Let's create a link in the document that we'll
            // programmatically 'click'.
            const link = document.createElement('a');
    
            // Tell the browser to associate the response data to
            // the URL of the link we created above.
            link.href = window.URL.createObjectURL(
                new Blob([response.data])
            );
    
            // Tell the browser to download, not render, the file.
            link.setAttribute('download', 'report.pdf');
    
            // Place the link in the DOM.
            document.body.appendChild(link);
    
            // Make the magic happen!
            link.click();
        }); // Please catch me!
    },
  
    async getInsInfo() {
      //alert(this.data.claim);
      this.ICN = this.data.claim;
      this.selections.claim = this.data.claim;
      //debugger;
      if (this.ICN != "") {
        this.results = await M("getInsuranceOptionsFromClaim^GJGTEST", this.data);
        debugger;
        console.log("Results getting claims", this.results);
        if (this.results != undefined) {
          this.data.acct = this.results.data.ACCT;
          this.data.TR = this.results.data.TR;
          this.selections.ACCT = this.results.data.ACCT;
          this.selections.TR = this.results.data.TR;
         
          //
          this.data.FormOptions = this.results.data.FormOptions;
          this.tempString = this.results.data.FormOptions;
          console.log("Temp String: ", this.tempString);
          //this.formArray = this.tempString.split("#");
          //
          this.data.InsOptions = this.results.data.InsOptions;
          this.tempString = this.results.data.InsOptions;
          //this.insArray = this.tempString.split("#");

          this.data.SelectedForm = this.results.data.SelectedForm;
          this.data.SelectedIns = this.results.data.SelectedIns;
          console.log("Insurance Array", this.insArray);
          console.log("Data:", this.results.data.InsOptions);
        }
        //alert("Data = ", this.results);
      }
    },
  },
};
</script>
<style scoped>
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
</style>
