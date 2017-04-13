<template>
  <f7-page>
    <f7-navbar title="Bug Info" back-link="Back" sliding>
      <f7-nav-right>
        <f7-link icon-size="22"  @click="saveBug()">save</f7-link>
      </f7-nav-right>
    </f7-navbar>
    <f7-block-title> {{ subject }} </f7-block-title>
    <f7-list form>
      <f7-list-item>
        <f7-label>Bug No:</f7-label>
        <f7-input type="text" placeholder="Name" v-model="bugNo">{{bugInfo.bugNo}}</f7-input>
      </f7-list-item>

      <f7-list-item>
        <f7-label>Filed By:</f7-label>
        <f7-input type="text" >{{bugInfo.filedByName}}</f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Assigned:</f7-label>
        <f7-input type="text"  v-model="assigned"></f7-input>
      </f7-list-item>
      <f7-list-item smart-select>
        <f7-label>Product:</f7-label>
        <!-- Select with values inside -->
        <select name="productId" v-model="productId" :selected="bugInfo.productIdName"><label>bugInfom.productIdName</label>
          <option
                  v-for="p in product"
                  :data="p"
                  :value="p.id"
                  >{{p.description}}</option>
        </select>
      </f7-list-item>
      <f7-list-item smart-select>
        <f7-label>Status:</f7-label>
        <!-- Select with values inside -->
        <select name="statusId" v-model="statusId" >
          <option
                  v-for="s in status"
                  :data="s"
                  :value="s.id"
                   >{{s.description}}</option>
        </select>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Subject:</f7-label>
        <f7-input type="text" name="subject" v-model="subject"></f7-input>
      </f7-list-item>
      <f7-list-item>
        <f7-label>Comment:</f7-label>
        <f7-input type="textarea" name="comments" v-model="comments"></f7-input>
      </f7-list-item>

    </f7-list>

  </f7-page>
</template>

<script>
    export default {
        data: function(){
            return {
                bugInfo:"",
                status:[],
                product:[],
                userId:"",

                bugNo:"",
                statusId:" ",
                productId:"",
                assigned:"",
                subject:"",
                comments:"",
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
                this.bugNo=response.data.bugNo;
                this.assigned=response.data.assignedName;
                this.productId=response.data.productId;
                this.statusId=response.data.statusId;
                this.subject=response.data.subject;

            },(response) => {
                console.log("bugInfo null");
            });

            this.$http({url: "findAllStatus", method: 'GET'}).then((response) =>
            {
                this.status = response.data;
            },(response) => {
                console.log("bugInfo null");
            });

            this.$http({url: "findAllProduct", method: 'GET'}).then((response) =>
            {
                this.product = response.data;
            },(response) => {
                console.log("bugInfo null");
            });
        },
        methods:{
            saveBug: function(){
              let urlpath="updateBug/?status="+this.statusId+"&assigned="+this.assigned+"&product="+this.productId+
                      "&subject="+this.subject+"&bugNo="+this.bugInfo.bugNo+"&comments="+this.comments+"&userId="+this.userId;
                console.log("update/?status="+this.statusId+"&assigned="+this.assigned+"&product="+this.productId+
                    "&subject="+this.subject+"&bugNo="+this.bugInfo.bugNo+"&comments="+this.comments+"&userId="+this.userId);
                this.$http({url:urlpath, method: 'GET'}).then((response) =>
                {
                    this.$f7.alert("Success!");
                },(response) => {
                    console.log("bugInfo null");
                });

            }
        }
    }
</script>
