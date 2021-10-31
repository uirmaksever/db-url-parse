<template>
  <b-container fluid class="px-0 h-100">
    <Navbar/>
    <div class="mx-auto mt-5" style="max-width: 600px">
      <InfoPanel/>

      <b-form>
        <b-form-group
          label="Enter DB URL"
          label-for="db_url_input"
        >
          <b-form-input
            id="db_url_input"
            v-model="db_url"
            placeholder="postgresql://[user[:password]@][netloc][:port][/dbname][?param1=value1&...]"
          ></b-form-input>

          
        </b-form-group>
      </b-form>
      <div>
        <b-alert v-if="error === true && isEmpty === false">
          {{ error_message }}
        </b-alert>
        <p>{{ new_db_url }}</p>
        <div><strong>DB Type: </strong>{{ db_url_parsed.driver }}
          <b-list-group>
            <b-list-group-item :key="index" v-for="(value, index) in db_parameters">
              <strong>{{ index }}</strong>
              <!-- <input :value="value"></input> -->
              <ClicktoEditFormField :db_parameter_value="value" v-on:update_db_parameter_value="onDBPerimeterUpdate(index, $event)" />
            </b-list-group-item>
          </b-list-group>
        </div>
      </div>
    </div>

  </b-container>
</template>

<script>
import {default as parseDbUrl} from "parse-database-url";

export default {

  data: function () {
    return {
      db_url: "",
      error: false,
      error_message: null,
      db_parameters: {},
    }
  },
  computed: {
    db_url_parsed: function () {
      try {
        var parsed_config =  parseDbUrl(this.db_url);
        this.db_parameters = parsed_config;
        console.log(parsed_config)
      }
      catch (error) {
        this.error = true;
        this.error_message = error.message;
        parsed_config = "";
        return parsed_config;
      }
      finally {
        return parsed_config
      }
    },
    new_db_url: function () {
      var url = "jdbc:" + this.db_parameters["driver"] +
      "://" + this.db_parameters["host"] +
      ":" +
      this.db_parameters["port"] 
      "/"

      return url
    },
    isEmpty: function () {
      if (this.db_url === "") {
        return True;
      } else {
        return False;
      }
    }
  },
  methods: {
    onDBPerimeterUpdate: function (parameter_index, new_db_parameter_value) {
      this.db_parameters[parameter_index] = new_db_parameter_value;
      console.log(parameter_index, new_db_parameter_value);
    },
  }
}
</script>
