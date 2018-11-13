<template>
  <section class="container">
    <Header>MyCSF Account Provisioning</Header>
    <form autocomplete="_away">
    <input type="hidden" name="auto" autocomplete="_away">
    <div class="box teal lighten-5">

      <!--First Row -->
      <div class="row">
        <div class="col m6 s12 input-field">
          <label for="remedy">Remedy Case(s)</label>
          <input type="text" name="remedy" id="remedy" v-model="remedy" placeholder="">
        </div>
        <div class="col m6 s12 input-field formType">
          <select name="formType" id="formType" v-model="type" v-on:change="prefillAssessor">
            <option value="">Select a Form Type</option>
            <option v-for="(type, index) in formType" :value="type.value" :key="index">{{ type.name }}</option>
          </select>
          <label for="formType">Form Type</label>
        </div>
        <div class="col m6 s12 input-field">
          <label for="orgname">Organization Name</label>
          <input type="text" name="orgname" id="orgname" placeholder="" v-model="orgname"/>
        </div>
        <div class="col m6 s12 input-field">
          <label for="accountnum">Account Number</label>
          <input type="text" name="accountnum" id="accountnum" placeholder="" v-model="accountnum"/>
        </div>
      </div>
    </div>


      <!--Second Row-->
      <div class="box red lighten-5">
      <div class="row">
        <div class="col m6 s12 input-field subscriptionType">
          <select name="subscriptionType" id="subscriptionType" v-model="subscription" v-on:change="prefillSubscription">
            <option value="" >Select a Subscription Type</option>
            <option v-for="(type,index) in subscriptionTypes" :key="index" :value="type.value">{{ type.name }}</option>
          </select>
          <label for="subscriptionType">Subscription Type</label>
        </div>
      </div>

      <!--Third Row-->
      <div class="row">
        <div class="col m6 s12 input-field">
          <label for="users"># of Users</label>
          <input type="number" name="users" id="users" v-model="users" placeholder="">
        </div>
        <div class="col m6 s12 input-field">
          <label for="objects"># of Objects</label>
          <input type="number" name="objects" id="objects" v-model="objects" placeholder="">
        </div>
      </div>

      <!--Fourth Row-->
      <div class="row">
        <div class="col m6 s12 input-field">
          <label for="credits"># of Report Credits</label>
          <input type="number" name="credits" id="credits" v-model="credits" placeholder="">
        </div>
        <div class="col m6 s12 input-field">
          <label for="expiration">Expiration Date</label>
          <input type="date" name="expiration" id="expiration" placeholder="">
        </div>
      </div>


      <!-- Fifth Row-->
      <div class="row">
        <div class="col m6 s12 input-field">
        <h6>Add-Ons</h6>
        <p>
        <label>
          <input type="checkbox" class="" v-model="reporting" />
          <span>Reporting</span>
        </label>
        </p>
        <p>
        <label>
          <input type="checkbox" class="" v-model="cap"/>
          <span>CAP Management</span>
        </label>
        </p>
        </div>
      </div>

      <!-- Assessor Row-->
      <div class="row">
        <div class="col m6 s12 input-field" id="assessorscheck">
          <h6>Assessor</h6>
          <p>
          <label>
            <input type="checkbox" class="" v-model="assessor" />
            <span>Include Assessor?</span>
          </label>
        </p>
      </div>
      </div>
      <div class="row" v-show="assessor">
      <div class="col m6 s12 input-field">
        <label for="assessmentname">Assessment Name</label>
        <input type="text" name="assessmentname" id="assessmentname" v-model="assessmentname" placeholder="">
      </div>

      <div class="col m6 s12 input-field">
        <label for="assessorname">Assessor Name</label>
        <input type="text" name="assessorname" id="assessorname" v-model="assessorname" placeholder="">
      </div>
    </div>
     </div>

    <!-- User Row -->
    <div class="box green lighten-5 user-box">
    <div class="row users">
      <h6 id="pad">Admins</h6>
      <div class="col s12 m6 input-field" v-on:keyup.enter="addAdmins">
        <label for="name">Name</label>
        <input type="text" name="name" id="name" placeholder="" v-model="adminname" autocomplete="_away">
      </div>
      <div class="col s12 m6 input-field" v-on:keyup.enter="addAdmins">
        <label for="email">Email</label>
        <input type="email" name="email" id="email" placeholder="" v-model="adminemail" autocomplete="_away">
      </div>
    </div>

    <!-- User Table -->
    <div class="row green lighten-5" v-show="admins.length > 0">
      <div><span class="bold">Remedy Case(s): </span><span id="remedy">{{remedy}}</span></div>
      <div><span class="bold">Account Number: </span>{{accountnum}}</div>
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Remove</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(admin,index) in admins" :key="index">
            <td>{{ admin.name }}</td>
            <td>{{ admin.email }}</td>
            <td v-on:click="removeAdmin"><i :id="index" class="material-icons cancel">cancel</i></td>
          </tr>
        </tbody>
      </table>
    </div>
    </div>

    </form>

    <div class="row signatureSection">
      <div class="col s12 m6 left">
        Name: <span class="bold">{{ name }}</span>
      </div>
      <div class="col s12 m6 left">
        Date: <span class="bold">{{ date }}</span>
      </div>
      <div class="col s12 m6 left signature">
        Signature: <input disabled type="text" name="signature" id="signature">
      </div>
    </div>
     <div class="row">
       <div class="col m6 s12">
         <button class="btn waves-effect waves-light" type="submit" name="print" v-on:click="printPage">Print
          <i class="material-icons right">print</i>
        </button>
        <button class="btn red waves-effect right" @click="resetForm">Reset
          <i class="material-icons right">replay</i>
        </button>
       </div>
     </div>
  </section>
</template>

<script>
import Header from '~/components/Header';

export default {
  components: {
    Header
  },
  data() {
    return {
      remedy: null,
      type: '',
      subscription: '',
      users: null,
      objects: null,
      credits: null,
      reporting: false,
      cap: false,
      orgname: null,
      accountnum: null,
      assessor: false,
      assessmentname: null,
      assessorname: null,
      adminname: null,
      adminemail: null,
      admins: [],
      formType: [
        {
          name: 'Account Form',
          value: 1
        },
        {
          name: 'Self-Assessment Form',
          value: 2
        },
        {
          name: 'Validated Assessment Form',
          value: 3
        },
        {
          name: 'Subscription User Form',
          value: 4
        },
        {
          name: 'Renewal Case',
          value: 5
        }
      ],
      subscriptionTypes: [
        {
          name: 'Report Only',
          value: 1
        },
        {
          name: 'Professional',
          value: 2
        },
        {
          name: 'Corporate',
          value: 3
        },
        {
          name: 'Premier',
          value: 4
        },
        {
          name: 'Assessor',
          value: 5
        }
      ]
    };
  },
  computed: {
    date() {
      let day = new Date();
      return day.getMonth() + '/' + day.getDate() + '/' + day.getFullYear();
    },
    name() {
      if (this.$route.query.name) {
        return this.$route.query.name;
      } else {
        return '';
      }
    }
  },
  methods: {
    resetForm() {
      if (confirm('Are you sure you wish to reset the form?')) {
        this.remedy = null;
        this.type = '';
        document.querySelector('.formType .select-dropdown ').value =
          'Select a Form Type';
        this.subscription = '';
        document.querySelector('.subscriptionType .select-dropdown ').value =
          'Select a Subscription Type';
        this.users = null;
        this.objects = null;
        this.credits = null;
        this.reporting = false;
        this.cap = false;
        this.orgname = null;
        this.accountnum = null;
        this.assessor = false;
        this.assessmentname = null;
        this.assessorname = null;
        this.adminname = null;
        this.adminemail = null;
        this.expiration = null;
        this.admins = [];
        document.querySelector('#expiration').value = '';
      }
    },
    prefillSubscription() {
      if (this.subscription === 1) {
        this.users = 3;
        this.objects = 1;
        this.reporting = false;
        this.cap = false;
        this.setReportDate();
      } else if (this.subscription === 2) {
        this.users = 5;
        this.objects = 2;
        this.reporting = true;
        this.cap = false;
        this.setSubscriptionDate();
      } else if (this.subscription === 3) {
        this.users = 10;
        this.objects = 5;
        this.reporting = true;
        this.cap = true;
        this.setSubscriptionDate();
      } else if (this.subscription === 4) {
        this.users = 25;
        this.objects = 10;
        this.reporting = true;
        this.cap = true;
        this.setSubscriptionDate();
      } else if (this.subscription === 5) {
        this.users = 60;
        this.objects = null;
        this.reporting = false;
        this.cap = false;
        document.querySelector('#expiration').value = '';
      }
    },
    prefillAssessor() {
      if (this.type === 3) {
        this.assessor = true;
      } else {
        this.assessor = false;
      }
    },
    addAdmins() {
      if (this.adminname && this.adminemail) {
        this.admins.push({
          name: this.adminname,
          email: this.adminemail
        });
        this.adminname = null;
        this.adminemail = null;
      }
    },
    removeAdmin(event) {
      this.admins.splice(parseInt(event.srcElement.id), 1);
    },
    setReportDate() {
      let today = new Date();
      today.setDate(today.getDate() + 91);
      let month =
        (today.getMonth() + 1).toString().length === 1
          ? '0' + (today.getMonth() + 1).toString()
          : (today.getMonth() + 1).toString();
      document.querySelector('#expiration').value =
        today.getFullYear().toString() +
        '-' +
        month +
        '-' +
        today.getDate().toString();
    },
    setSubscriptionDate() {
      let today = new Date();
      if (today.getFullYear % 4 === 0) {
        today.setDate(today.getDate() + 367);
      } else {
        today.setDate(today.getDate() + 366);
      }
      let month =
        (today.getMonth() + 1).toString().length === 1
          ? '0' + (today.getMonth() + 1).toString()
          : (today.getMonth() + 1).toString();
      document.querySelector('#expiration').value =
        today.getFullYear().toString() +
        '-' +
        month +
        '-' +
        today.getDate().toString();
    },
    printPage() {
      window.print();
    }
  },
  beforeMount() {
    document.addEventListener('DOMContentLoaded', function() {
      var elems = document.querySelectorAll('select');
      var instances = M.FormSelect.init(elems, {});
    });

    document.addEventListener('DOMContentLoaded', function() {
      var elems = document.querySelectorAll('.datepicker');
      var instances = M.Datepicker.init(elems, {
        format: 'mm/dd/yyyy'
      });
    });
  },
  head() {
    return {
      title: 'MyCSF Provisioning'
    };
  }
};
</script>

<style scoped>
#remedy {
  font-style: italic;
}

.space-above {
  margin-top: 100px;
}

.box {
  border: 1px dotted black;
  padding: 10px;
  margin: 30px 0;
}

.signature {
  margin: 25px 0 0 0;
}

.bold {
  font-weight: bold;
}

#pad {
  padding-left: 0.75em;
}

.signatureSection {
  margin-top: 100px;
  margin-bottom: 50px;
}

.cancel {
  cursor: pointer;
}

@media print {
  * {
    -webkit-print-color-adjust: exact;
  }

  button,
  .caret,
  .users,
  td:nth-child(3),
  th:nth-child(3),
  #assessorscheck {
    display: none;
  }

  .box {
    border: none;
    -webkit-print-color-adjust: exact;
  }

  .user-box {
    background-color: white !important;
  }
}
</style>
