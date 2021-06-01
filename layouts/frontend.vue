<template>
  <div>
    <b-navbar toggleable="lg" type="light" fixed="top" class="bg-diary">
      <div class="container">
        <b-col md="12">
          <b-collapse id="nav-collapse" is-nav>
            <NuxtLink to="/" class="navbar-brand"><b>Diary</b></NuxtLink>

            <!-- Right aligned nav items -->
            <b-navbar-nav class="ml-auto">
              <b-nav-form>
                <b-form-input
                  size="sm"
                  class="mr-sm-2 d-sm-none d-md-block"
                  placeholder="Search"
                ></b-form-input>
                <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button
                >
              </b-nav-form>
            </b-navbar-nav>
          </b-collapse>
        </b-col>
      </div>
    </b-navbar>
    <Nuxt class="content" />
    <!-- keep-alive akan menyimpan data fetch ke memory -->

    <!-- <nuxt keep-alive :keep-alive-props="{ max: 10 }" /> yg disimpan maximal 10 komponen-->
  </div>
</template>

<style>
.content {
  margin-top: 80px !important;
}
</style>
