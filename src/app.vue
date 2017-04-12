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
            <f7-navbar v-if="$theme.material" title="Left Panel" sliding></f7-navbar>
            <f7-block-title>About Me</f7-block-title>
            <f7-list>
              <f7-list-item link="/form/" title="About Me"></f7-list-item>
              <f7-list-item link="/form/" title="Setting"></f7-list-item>
            </f7-list>
            <f7-block-title>Features</f7-block-title>
            <f7-list>
              <f7-list-item link="/form/" title="Open Bugs for My ORG" link-view="#main-view" link-close-panel></f7-list-item>
              <f7-list-item link="/form/" title="Open Bugs Assigned to Me" link-view="#main-view" link-close-panel></f7-list-item>
              <f7-list-item link="/form/" title="Open Bugs Filed by Me" link-view="#main-view" link-close-panel></f7-list-item>
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
              <f7-tab id="assigned"  @tab:show="tabActived('assigned')" >
                <bug-view :userId="userid"></bug-view>
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
            <f7-preloader></f7-preloader>
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
              <f7-list-item>
                <f7-label>productId</f7-label>
                <f7-input type="text" placeholder="Name"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label >component</f7-label>
                <f7-input type="text"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label >status</f7-label>
                <f7-input type="text"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label >assigned</f7-label>
                <f7-input type="text"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label >severity</f7-label>
                <f7-input type="text"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label >tag</f7-label>
                <f7-input type="text"></f7-input>
              </f7-list-item>
              <f7-list-item>
                <f7-label >filedBy</f7-label>
                <f7-input type="text"></f7-input>
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
    import AboutView  from './pages/about'
    var timeout;
    export default {
        name: 'app',
        data(){
            return {
                username: "",
                password: "",
                activedTab:"home",
                infiniteScrollPreloader: false
            }
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
                        this.$refs.org.advancedSearch();
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
            }
        },
        components: {
            BugView,
            AboutView
        }
    }
</script>
<!--

-->
