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
        <div><strong>DB Type: </strong>{{ db_url_parsed.driver }}
          <b-list-group>
            <b-list-group-item :key="index" v-for="(index, value) in db_url_parsed">
              <strong>{{ value }}</strong> <span class="mx-auto"></span> <code>{{ index }}</code>
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
    }
  },
  computed: {
    db_url_parsed: function () {
      try {
        var parsed_config =  parseDbUrl(this.db_url);
        console.log(parsed_config)
      }
      catch (error) {
        this.error = true;
        this.error_message = error.message
        parsed_config = ""
        return parsed_config
      }
      finally {
        return parsed_config
      }
    },
    isEmpty: function () {
      if (this.db_url === "") {
        return True;
      } else {
        return False;
      }
    }
  },
}
</script>
