<template>
  <!-- App -->
  <div id="app">

    <!-- Statusbar -->
    <f7-statusbar></f7-statusbar>
    <!-- Left Panel -->
    <f7-panel left reveal layout="dark">
      <f7-view id="left-panel-view" navbar-through :dynamic-navbar="true">
        <f7-navbar v-if="$theme.ios" title="Left Panel" sliding></f7-navbar>
        <f7-pages>
          <f7-page>
            <f7-block-title>Graph status</f7-block-title>
            <f7-list>
              <f7-list-item link="/graph/" title="Graph Summary" link-view="#main-view" link-close-panel></f7-list-item>
            </f7-list>
          </f7-page>
        </f7-pages>
      </f7-view>
    </f7-panel>
    <!-- Main Views -->
    <f7-views>
      <f7-view id="main-view" navbar-through :dynamic-navbar="true" main>
        <f7-navbar v-if="$theme.ios">
          <f7-nav-left>
            <f7-link icon="icon-bars" open-panel="left"></f7-link>
          </f7-nav-left>
          <f7-nav-center sliding>BugDB</f7-nav-center>
        </f7-navbar>

        <!-- Pages -->
        <f7-pages>
          <f7-page toolbar-fixed navbar-fixed infinite-scroll @infinite="onInfinite" :infinite-scroll-preloader="false">
            <f7-toolbar tabbar labels>
              <f7-link icon-f7="home" text="Org" tab-link="#home" active></f7-link>
              <f7-link icon-f7="data" text="Filed" tab-link="#filed"></f7-link>
              <f7-link icon-f7="world" text="Assigned" tab-link="#assigned"></f7-link>
              <f7-link icon-f7="settings" text="setting"  tab-link="#about"></f7-link>
            </f7-toolbar>
            <f7-tabs>
              <f7-tab id="home" active @tab:show="tabActived('home')">
                <bug-view :userId="userid" ref="org"></bug-view>
              </f7-tab>
              <f7-tab id="filed"  @tab:show="tabActived('filed')">
                <bug-view :userId="userid"></bug-view>
              </f7-tab>
              <f7-tab id="assigned"  @tab:show="tabActived('assigned')">
                <assigned-view :userId="userid"></assigned-view>
              </f7-tab>
              <f7-tab id="about">
                <about-view></about-view>
              </f7-tab>
            </f7-tabs>
          </f7-page>
        </f7-pages>
      </f7-view>
    </f7-views>

    <!-- Login Screen -->
    <f7-login-screen id="login-screen" opened>
      <f7-view>
        <f7-pages>
          <f7-page login-screen>
            <f7-login-screen-title>Login</f7-login-screen-title>
            <f7-list form>
              <f7-list-item>
                <f7-label>Username</f7-label>
                <f7-input name="username" v-model="username" placeholder="Username" type="text"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label>Password</f7-label>
                <f7-input name="password" v-model="password" type="password" placeholder="Password"></f7-input>
              </f7-list-item>
            </f7-list>
            <f7-list>
              <f7-list-button title="Sign In" @click="login"></f7-list-button>
              <f7-list-label>
                <p>Click Sign In to close Login Screen</p>
              </f7-list-label>
            </f7-list>
          </f7-page>
        </f7-pages>
      </f7-view>
    </f7-login-screen>

    <f7-popup id="advanced" @popup:closed="go2search">
      <f7-view navbar-fixed>
        <f7-pages>
          <f7-page>
            <f7-navbar title="Advanced Search">
              <f7-nav-right>
                <f7-link close-popup>Search</f7-link>
              </f7-nav-right>
            </f7-navbar>

            <f7-list form>
              <f7-list-item smart-select>
                <f7-label>Product:</f7-label>
                <!-- Select with values inside -->
                <select name="product" v-model="product">
                  <option
                          v-for="p in allproduct"
                          :data="p"
                          :value="p.id"
                  >{{p.id+"."+p.description}}</option>
                </select>
              </f7-list-item>

              <f7-list-item smart-select>
                <f7-label>Component:</f7-label>
                <!-- Select with values inside -->
                <select name="component" v-model="component" >
                  <option
                          v-for="c in allcomponent"
                          :data="c"
                          :value="c.id"
                  >{{c.id+"."+c.description}}</option>
                </select>
              </f7-list-item>

              <f7-list-item smart-select>
                <f7-label>Status:</f7-label>
                <!-- Select with values inside -->
                <select name="status" v-model="status" >
                  <option
                          v-for="s in allstatus"
                          :data="s"
                          :value="s.id"
                  >{{s.id+"."+s.description}}</option>
                </select>
              </f7-list-item>

              <f7-list-item>
                <f7-label >assigned</f7-label>
                <f7-input type="text" v-model="assigned"></f7-input>
              </f7-list-item>

              <f7-list-item smart-select>
                <f7-label>severity:</f7-label>
                <!-- Select with values inside -->
                <select name="severity" v-model="severity" >
                  <option
                          v-for="e in allseverity"
                          :data="e"
                          :value="e.id"
                  >{{e.id+"."+e.description}}</option>
                </select>
              </f7-list-item>

              <f7-list-item>
                <f7-label >tag</f7-label>
                <f7-input type="text" v-model="tag"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label >filedBy</f7-label>
                <f7-input type="text" v-model="filedBy"></f7-input>
              </f7-list-item>

              <f7-list-item>
                <f7-label>start time</f7-label>
                <f7-input name="starttime" type="datetime-local" v-model="starttime"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label>end time</f7-label>
                <f7-input name="endtime" type="datetime-local" v-model="endtime"></f7-input>
              </f7-list-item>
              <f7-list-item>
               </f7-list-item>
            </f7-list>
            <f7-list>
              <f7-list-item>
            <f7-label >Criteria Name</f7-label>
            <f7-input type="text" v-model="searchname"></f7-input>
                </f7-list-item>
            </f7-list>
            <f7-grid>
             <f7-col width="1%"></f7-col>
             <f7-col><f7-button raised color="blue"  @click="saveQuery">Save criteria</f7-button></f7-col>
             <f7-col><f7-button raised color="blue"  @click="resetForm">Reset</f7-button></f7-col>
            </f7-grid>


            <f7-block-title>Searh history</f7-block-title>
            <f7-list accordion>
              <f7-list-item accordion-item
                v-for="h in historyList"
                :key="h"
                :title="h.searchName+' '+h.id "
                            @click="loadhistory(h.id)"
                @accordion:open="onOpen"
                @accordion:opened="onOpened"
                @accordion:close="onClose"
                @accordion:closed="onClosed"
              >
                <f7-accordion-content v-for="h in historyList">
                  <f7-block @click="loadhistory(h.id)">

                  </f7-block>
                </f7-accordion-content>
              </f7-list-item>
            </f7-list>

          </f7-page>
        </f7-pages>
      </f7-view>
    </f7-popup>

  </div>
</template>

<script>
    import BugView from './pages/buglist'
    import AssignedView from './pages/assigned'
    import AboutView  from './pages/about'
    var timeout;
    export default {
        name: 'app',
        data(){
            return {
                username: "",
                password: "",
                activedTab:"home",
                infiniteScrollPreloader: false,
                allstatus:"",
                allproduct:"",
                allseverity:"",
                allcomponent:"",
                //advancedSearch
                product:"",
                component:"",
                status:"",
                assigned:"",
                severity:"",
                tag:"",
                filedBy:"",
                starttime:"",
                endtime:"",
                //queryHistory
                historyList:"",
                searchname:""

            }
        },
        mounted(){
            this.$http({url: "findAllStatus", method: 'GET'}).then((response) =>
            {
                this.allstatus = response.data;
            },(response) => {
                console.log("bugInfo null");
            });

            this.$http({url: "findAllProduct", method: 'GET'}).then((response) =>
            {
                this.allproduct = response.data;
            },(response) => {
                console.log("bugInfo null");
            });
            this.$http({url: "findAllSeverity", method: 'GET'}).then((response) =>
            {
                this.allseverity = response.data;
            },(response) => {
                console.log("bugInfo null");
            });
            this.$http({url: "findAllComponent", method: 'GET'}).then((response) =>
            {
                this.allcomponent = response.data;
            },(response) => {
                console.log("bugInfo null");
            });
            let findsearch="findSearchHistory?userId="+userid;
            this.$http({url: findsearch, method: 'GET'}).then((response) =>
            {
                this.historyList = response.data;
                console.log(this.historyList);
            },(response) => {
                console.log("history failed");
            });
        },
        computed: {
            // a computed getter
            userid: function () {
                // `this` points to the vm instance
                return this.$localStorage.get('userid');
            }
        },
        methods:{
            tabActived(tab) {
                this.activedTab = tab
            },
            loadhistory: function(query){
              let url1="findSearchHistoryById?id="+query;
                this.$http({url:url1, method: 'GET'}).then((response) =>
                {

                    let res=response.data;
                    this.product=res.product;
                    this.component=res.componentId;
                    this.status=res.status;
                    this.assigned=res.assigned;
                        this.severity=res.severity;
                        this.tag=res.tag;
                        this.filedBy=res.filedBy;
                },(response) => {
                    console.log("save failed");
                });
            },
            saveQuery: function(){

            },
            resetForm: function(){

            },
            onInfinite: function(event){

                switch(this.activedTab) {
                    case 'home':
                        console.log("home");
                        this.$refs.org.loadOrg();
                    case 'filed':
                        this.$refs.org.loadOrg();
                    case 'assigned':
                        this.$refs.org.loadOrg();
                }
            },
            go2search: function(){
                switch(this.activedTab) {
                    case 'home':
                        console.log("home");
                        let urlpath="advancedSearch/?productId="+this.product+"&component="+this.component+"&status="+this.status+"&assigned="
                            +this.assigned+"&severity="+this.severity+"&tag="+this.tag+"&filedBy="+this.filedBy+"&startTime="+this.starttime
                        +"&endTime="+this.endtime;
                        let obj=new Object();
                        this.$refs.org.advancedSearch(urlpath);
                    case 'filed':
                        this.$refs.org.advancedSearch();
                    case 'assigned':
                        this.$refs.org.advancedSearch();
                }
            },
            login: function(){
                if(this.username != '' && this.password !=''){
                    this.toLogin();
                }
            },
            toLogin(){
                let loginParam = {
                    username: this.username,
                    password: this.password
                }
                this.$http({url: "tokens", body: loginParam, method: 'POST'}).then((response) =>
                {
                    this.$f7.showPreloader("loading");
                    var data = response.data;
                    if(data.code == 100){
                        this.$localStorage.set('token', data.content.token);
                        this.$localStorage.set('userid',data.content.userId);
                        console.log(data.content.token);
                        this.$f7.hidePreloader();
                        this.$f7.closeModal("#login-screen");
                    }
                },(response) => {
                    console.log("invalid username or password");
                });
            },
            resetForm(){
                this.product=""
                this.component="",
                    this.status="",
                    this.assigned="",
                    this.severity="",
                    this.tag="",
                    this.filedBy="",
                    this.starttime=null,
                    this.endtime=null
            },
            saveQuery(){
                console.log(this.userid+"az");
                let urlsave="saveSearchHistory?id="+"&searchName="+this.searchname+"&productId="+this.product+"&component="+this.component+"&status="+this.status+"&assigned="
                    +this.assigned+"&severity="+this.severity+"&tag="+this.tag+"&filedBy="+this.filedBy+"&startTime="+this.starttime
                    +"&endTime="+this.endtime+"&userId="+this.userid;
                console.log(urlsave);
                this.$http({url:urlsave, method: 'GET'}).then((response) =>
                {
                    let findsearch="findSearchHistory?userId="+1;
                    this.$http({url: findsearch, method: 'GET'}).then((response) =>
                    {
                        this.historyList = response.data;
                        console.log(this.historyList);
                    },(response) => {
                        console.log("history failed");
                    });
                    this.$f7.alert("Success!");
                },(response) => {
                    console.log("save failed");
                });
            }
        },
        components: {
            BugView,
            AboutView,
            AssignedView
        }
    }
</script>
<!--

-->
