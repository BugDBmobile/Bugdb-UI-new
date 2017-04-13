<template>
    <div class="bug-view">
        <f7-searchbar cancel-link="Cancel" search-list="#bug-list" customSearch
                      @searchbar:search="onSearch" @searchbar:enable="onEnable" found="#search-found"
                      @searchbar:disable="onDisable" @searchbar:clear="onClear" @search:keypress.enter="record">
            <div slot="before-input"  >
                <select v-model="selected" @change="onChange">
                    <option >bugId</option>
                    <option >bugText</option>
                    <option >advanced</option>
                </select>
            </div>
        </f7-searchbar>
        <f7-list class="searchbar-not-found">
            <f7-list-item title="Nothing found"></f7-list-item>
        </f7-list>
        <f7-list media-list class="searchbar-found" id="bug-list">
            <f7-list-item
                    v-for="(data, index) in bugdata" v-if="data!==undefined && data!=null"
                    :key="index"
                    :title="'Bug No: ' + data.bugNo"
                    :subtitle="'Time ' + data.filed"
                    :data="data"
                    :text="data.subject"
                    @click="getBugInfo(data)"
                    :badge="data.severityId" badge-color="red"
            ></f7-list-item>
            <!-- 加载提示符 -->
            <div id="list-pre" class="infinite-scroll-preloader" v-show="showPreloader">
                <div class="preloader"></div>
            </div>
        </f7-list>

        <!--infinite-scroll @infinite="onInfinite"
            <card v-for="(item, index) in timeline" :key="item.id" :data="item" @card:content-click="routeToPost"></card>
          -->
    </div>
</template>
<style>
    .infinite-scroll-preloader {
        margin-bottom: 1px;
        text-align: center;
    }
    .infinite-scroll-preloader .preloader {
        width:34px;
        height:34px;
    }
</style>
<script>
    var timeout;
    export default {
        data: function () {
            return {
                counter: 6,
                showPreloader:false,
                bugdata:"",
                selected:"bugId",
                customSearch:true,
                resultnot:false,
                resultfound:true,
                urlpath:""
            }
        },
        computed: {

        },
        props :['userId'],
        mounted(){
            this.$http({url: "findByFiledBy", params:{userId: this.userId}, method: 'GET'}).then((response) =>
            {
                this.bugdata = response.data;
            },(response) => {
                console.log("bugdata null");
            });
        },
        methods: {
            loadOrg: function (){
                this.showPreloader = true;
                console.log("working loadorg");
                var self = this;
                clearTimeout(timeout);
                timeout = setTimeout(function () {
                    for (var i = self.counter; i < self.counter + 3; i++) {
                        self.items.push(i);
                    }
                    self.counter = i;
                    this.showPreloader = false;
                }, 1000);
            },
            onChange: function(){
                if(this.selected == 'advanced'){
                    console.log("here");
                    this.$f7.popup("#advanced");
                }
            },
            advancedSearch: function(query){
                this.urlpath=query;
                console.log(this.urlpath);
                this.$http({url:this.urlpath, method: 'GET'}).then((response) =>
                {
                    let data = response.data;
                    console.log(data);
                    this.bugdata = data;
                },(response) => {
                    this.bugdata = null;
                });

                this.selected = "bugId";
            },
            record: function(){
                console.log("Enter printed");
            },
            onSearch: function (query, found) {
                console.log(found);
                switch(this.selected) {
                    case 'bugId':
                        this.$http({url: "findByBugNo", params:{bugNo: query}, method: 'GET'}).then((response) =>
                        {
                            let data = new Array(response.data);
                            this.bugdata = data;
                        },(response) => {
                            this.bugdata = null;
                        });
                        break;
                    case 'bugText':
                        this.$http({url: "esbug/"+query, method: 'GET'}).then((response) =>
                        {
                            this.bugdata = response.data;
                        },(response) => {
                            this.bugdata = null;
                        });
                        break;
                }
                console.log('search', query);
            },
            onClear: function (event) {
                console.log('clear');
            },
            onEnable: function (event) {
                console.log('enable');
            },
            onDisable: function (event) {
                this.$http({url: "findByAssigned", params:{userId: this.userId}, method: 'GET'}).then((response) =>
                {
                    this.bugdata = response.data;
                    this.resultnot = false;
                    this.resultfound = true;
                },(response) => {
                    console.log("bugdata null");
                });
            },
            getBugInfo(data) {
                this.$f7.mainView.router.load({url:`/form/?bugId=${data.bugNo}&userId=${this.userId}`});
            }
        }
    }
</script>
