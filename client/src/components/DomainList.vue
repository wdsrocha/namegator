<template>
  <div>
    <div id="main">
      <div class="container">
        <div class="row">
          <div class="col-md">
            <AppItemList
              title="Prefixos"
              v-bind:items="prefixes">
            </AppItemList>
          </div>
          <div class="col-md">
            <AppItemList
              title="Sufixos"
              v-bind:items="suffixes">
            </AppItemList>
          </div>
        </div>
        <br/>
        <h5>Domínios <span class="badge badge-info">{{ domains.length }}</span></h5>
        <div class="card">
          <div class="card-body">
            <ul class="list-group">
              <li class="list-group-item" v-for="domain in domains" v-bind:key="domain.name">
                <div class="row">
                  <div class="col-md">
                    {{ domain.name }}
                  </div>
                  <div class="col-md text-right">
                    <a class="btn btn-info" v-bind:href="domain.checkout" target="_blank">
                      <span class="fa fa-shopping-cart"></span>
                    </a>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios/dist/axios";
import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";
import AppItemList from "./AppItemList";

export default {
  name: "DomainList",
  components: {
    AppItemList
  },
  data: function () {
    return {
      prefixes: [],
      suffixes: [],
    };
  },
  computed: {
    domains() {
      const domains = [];
      for (const prefix of this.prefixes) {
        for (const suffix of this.suffixes) {
          const name = prefix + suffix;
          const checkout = `https://checkout.hostgator.com.br/?a=add&sld=${name.toLowerCase()}&tld=.com`;
          domains.push({
            name,
            checkout
          });
        }
      }
      return domains;
    }
  },
  async created() {
    const response = await axios({
      url: "http://localhost:4000",
      method: "post",
      data: {
        query: `
          {
            prefixes: items (type: "prefix") {
              id
              type
              description
            }
            suffixes: items (type: "suffix") {
              description
            }
          }
        `
      }
    });
    const query = response.data;
    this.prefixes = query.data.prefixes.map(
      prefix => prefix.description);
    this.suffixes = query.data.suffixes.map(
      suffix  => suffix.description);
  }
};
</script>

<style>
</style>
