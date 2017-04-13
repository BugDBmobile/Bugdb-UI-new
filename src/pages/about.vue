<template>
  <div class="about-view">
  <f7-list>
    <f7-list-item>
      <f7-label>Name</f7-label>
      <f7-icon slot="media" f7="person"></f7-icon>
      <f7-input :disabled="true"> iceforge</f7-input>
    </f7-list-item>
    <f7-list-item>
      <f7-label>Email</f7-label>
      <f7-icon slot="media" f7="email"></f7-icon>
      <f7-input f7-input :disabled="true">bug.demo@oracle.com</f7-input>
    </f7-list-item>
    <f7-list-item>
      <f7-label>gender</f7-label>
      <f7-icon slot="media" f7="person"></f7-icon>
      <f7-input :disabled="true"> male</f7-input>
    </f7-list-item>
    <f7-list-item>
      <f7-label>phone</f7-label>
      <f7-icon slot="media" f7="person"></f7-icon>
      <f7-input :disabled="true">010-60166029</f7-input>
    </f7-list-item>
       <f7-list-button open-login-screen>LogOut</f7-list-button>
  </f7-list>
  </div>
</template>
<script>
export default {

}
</script>
