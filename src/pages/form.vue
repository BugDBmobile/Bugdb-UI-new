<template>
  <f7-page>
    <f7-navbar title="Bug Info" back-link="Back" sliding>
      <f7-nav-right>
        <f7-link   @click="updateBug()" >update</f7-link>
      </f7-nav-right>
    </f7-navbar>
    <f7-block-title></f7-block-title>
    <f7-list form>
      <f7-list-item>
        <f7-label>Bug No</f7-label>
        <f7-input type="text" placeholder="Name" :value="bugInfo.bugNo"></f7-input>
      </f7-list-item>

      <f7-list-item>
        <f7-label >Filed By:</f7-label>
        <f7-input type="text" >{{bugInfo.filedByName}}</f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Filed:</f7-label>
        <f7-input type="text" >{{bugInfo.filed}}</f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Assigned:</f7-label>
        <f7-input type="text" >{{bugInfo.assignedName}}</f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Customer:</f7-label>
        <f7-input type="text" >{{bugInfo.customer}}</f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>BugType:</f7-label>
        <f7-input type="text" >{{bugInfo.bugType}}</f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Status:</f7-label>
        <f7-input type="text">{{bugInfo.statusIdName}}</f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Fixed Ver:</f7-label>
        <f7-input type="text">{{bugInfo.fixedVer}}</f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Subject</f7-label>
        <div class="changeline">
              {{ bugInfo.subject }}
        </div>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Component</f7-label>
        <f7-input type="text" >{{bugInfo.componentIdName}}</f7-input>
      </f7-list-item>
    </f7-list>


    <f7-block-title>Comments</f7-block-title>
    <f7-block inner>
      <f7-accordion-item v-for="(data, index) in updates" :key="index">
        <f7-accordion-toggle><b class="fz">
        <f7-grid>
            <p>user:{{ data.userId }}   {{ data.time}}</p>
        </f7-grid>
        </b></f7-accordion-toggle>
        <f7-accordion-content>

        <f7-grid>
            <f7-col>{{ data.chg }} </f7-col>
        </f7-grid>
        <f7-grid>
            <f7-col>{{ data.comments }}</f7-col>
        </f7-grid>
        </f7-accordion-content>
      </f7-accordion-item>
    </f7-block>

<!--
<f7-block-title>Comments</f7-block-title>
<f7-list accordion>
  <f7-list-item accordion-item
    v-for="(data, index) in updates"
    :key="index"
    :title="data.userId+' '+data.time "
    @accordion:open="onOpen"
    @accordion:opened="onOpened"
    @accordion:close="onClose"
    @accordion:closed="onClosed"
  >
    <f7-accordion-content v-for="(data, index) in updates">
      <f7-block>
          <p>{{ data.chg }} </p>

          <p>{{ data.comments }}</p>

      </f7-block>
    </f7-accordion-content>
  </f7-list-item>
</f7-list>



<f7-block-title>Comments</f7-block-title>
<f7-block inner>
  <f7-accordion-item v-for="(data, index) in updates"
    title="data.userId+' '+ data.time">
    <f7-accordion-content>
    <f7-grid>
        <f7-col>{{ data.chg }} </f7-col>
    </f7-grid>
    <f7-grid>
        <f7-col>{{ data.comments }}</f7-col>
    </f7-grid>
    </f7-accordion-content>
  </f7-accordion-item>
</f7-block>
-->




  </f7-page>
</template>
<style>
.changeline {
   word-wrap: break-all;
   word-break: normal;
   width:150 px;
   text-align:left;
}

.fz {
  font-size:18px;
}
</style>
<script>
    export default {
        data: function(){
            return {
                bugInfo:"",
                user:"",
                updates:[],
                userId:""
            }
        },
        computed: {
            subject: function(){
                return  this.bugInfo.bugNo+ " " +this.bugInfo.subject;
            }
        },
        mounted(){
            let query = this.$route.query;
            let bugId = query["bugId"];
            this.userId=query["userId"];
            this.$http({url: "findByBugNo", params:{bugNo: bugId}, method: 'GET'}).then((response) =>
            {
                this.bugInfo = response.data;
            },(response) => {
                console.log("bugInfo null");
            });
            console.log(bugId)
            let url1="findUpdatesByBugNo/?bugNo="+bugId;
            this.$http({url: url1, method: 'GET'}).then((response) => {
                this.updates = response.data;
                console.log(response.data);
            }, (response) => {
                console.log("invalid username or password");
            });
        },

        methods:{
            updateBug: function(){
                let urlpath = "/update/?bugId="+ this.bugInfo.bugNo+"&userId="+this.userId;
                this.$f7.mainView.router.load({url: urlpath})
            },
            onOpen(){

            },
            onOpened(){

            },
            onClose(){

            },
            onClosed(){

            }
        }
    }
</script>
