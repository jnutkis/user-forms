<template>
  <section class="container">
    <Header>MyCSF Account Provisioning</Header>
    <div class="box teal lighten-5">
    <form>
      <!--First Row -->
      <div class="row">
        <div class="col m6 s12 input-field">
          <label for="remedy">Remedy Case(s)</label>
          <input type="text" name="remedy" id="remedy" v-model="remedy" placeholder="">
        </div>
        <div class="col m6 s12 input-field formType">
          <select name="formType" id="formType" v-model="type" v-on:change="prefillAssessor">
            <option disabled value="">Select a Form Type</option>
            <option v-for="(type, index) in formType" :value="type.value" :key="index">{{ type.name }}</option>
          </select>
          <label for="formType">Form Type</label>
        </div>
      </div>


      <!--Second Row-->
      <div class="row space-above">
        <div class="col m6 s12 input-field">
          <label for="orgname">Organization Name</label>
          <input type="text" name="orgname" id="orgname" placeholder="" v-model="orgname"/>
        </div>
        <div class="col m6 s12 input-field">
          <label for="accountnum">Account Number</label>
          <input type="text" name="accountnum" id="accountnum" placeholder="" v-model="accountnum"/>
        </div>
      </div>


      <!--Second Row-->
      <div class="row">
        <div class="col m6 s12 input-field subscriptionType">
          <select name="subscriptionType" id="subscriptionType" v-model="subscription" v-on:change="prefillSubscription">
            <option disabled value="" >Select a Subscription Type</option>
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
      </div>


      <!-- Fifth Row-->
      <div class="row">
        <div class="col m6 s12 input-field">
        <h6>Add-Ons</h6>
        <p>
        <label>
          <input type="checkbox" class="filled-in" v-model="reporting" />
          <span>Reporting</span>
        </label>
        </p>
        <p>
        <label>
          <input type="checkbox" class="filled-in" v-model="cap"/>
          <span>CAP Management</span>
        </label>
        </p>
        </div>
      </div>

      <!-- Assessor Row-->
      <div class="row">
        <div class="col m6 s12 input-field">
          <h6>Assessor</h6>
          <p>
          <label>
            <input type="checkbox" class="filled-in" v-model="assessor" />
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

    </form>

    <div class="row">
      <div class="col s12 m6 left">
        Name: {{name}}
      </div>
      <div class="col s12 m6 left">
        Date: {{ date }}
      </div>
      <div class="col s12 m6 left signature">
        Signature:
      </div>
    </div>
    <button class="btn red waves-effect" @click="resetForm">Reset</button>
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
      type: undefined,
      subscription: undefined,
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
      this.remedy = null;
      this.type = undefined;
      document.querySelector('.formType .select-dropdown ').value =
        'Select a Form Type';
      this.subscription = undefined;
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
    },
    prefillSubscription() {
      if (this.subscription === 1) {
        this.users = 3;
        this.objects = 1;
        this.reporting = false;
        this.cap = false;
      } else if (this.subscription === 2) {
        this.users = 5;
        this.objects = 2;
        this.reporting = true;
        this.cap = false;
      } else if (this.subscription === 3) {
        this.users = 10;
        this.objects = 5;
        this.reporting = true;
        this.cap = true;
      } else if (this.subscription === 4) {
        this.users = 25;
        this.objects = 10;
        this.reporting = true;
        this.cap = true;
      } else if (this.subscription === 5) {
        this.users = 60;
        this.objects = null;
        this.reporting = false;
        this.cap = false;
      }
    },
    prefillAssessor() {
      if (this.type === 3) {
        this.assessor = true;
      } else {
        this.assessor = false;
      }
    }
  },
  beforeMount() {
    document.addEventListener('DOMContentLoaded', function() {
      var elems = document.querySelectorAll('select');
      var instances = M.FormSelect.init(elems, {});
    });
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
  border: 1px solid black;
  padding: 10px;
}

.signature {
  margin: 25px 0 0 0;
}

@media print {
  button,
  .caret {
    display: none;
  }
}
</style>
