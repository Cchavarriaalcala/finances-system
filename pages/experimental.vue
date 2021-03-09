<template>
  <div class="container">
    <div class="panel panel-sm">
      <div class="panel-heading">
        <h4>CSV Import</h4>
      </div>
      <div class="panel-body">
        <div class="form-group">
          <label for="csv_file" class="control-label col-sm-3 text-right"
            >CSV file to import</label
          >
          <div class="col-sm-9">
            <input
              id="csv_file"
              type="file"
              name="csv_file"
              class="form-control"
              @change="loadCSV($event)"
            />
          </div>
        </div>
        <div class="col-sm-offset-3 col-sm-9">
          <div class="checkbox-inline">
            <label for="header_rows"
              ><input id="header_rows" type="checkbox" /> File contains header
              row?</label
            >
          </div>
        </div>

        <div class="col-sm-offset-3 col-sm-9">
          <a href="#" class="btn btn-primary">Parse CSV</a>
        </div>
        <table v-if="parse_csv">
          <thead>
            <tr>
              <th
                v-for="key in parse_header"
                :key="key"
                :class="{ active: sortKey == key }"
                @click="sortBy(key)"
              >
                {{ key | capitalize }}
                <span
                  class="arrow"
                  :class="sortOrders[key] > 0 ? 'asc' : 'dsc'"
                >
                </span>
              </th>
            </tr>
          </thead>
          <tr v-for="csv in parse_csv" :key="csv">
            <td v-for="key in parse_header" :key="key">
              {{ csv[key] }}
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Index',
  filters: {
    capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    },
  },
  data() {
    return {
      channel_name: '',
      channel_fields: [],
      channel_entries: [],
      parse_header: [],
      parse_csv: [],
      sortOrders: {},
      sortKey: '',
    }
  },
  methods: {
    sortBy(key) {
      const vm = this
      vm.sortKey = key
      vm.sortOrders[key] = vm.sortOrders[key] * -1
    },
    csvJSON(csv) {
      const vm = this
      const lines = csv.split('\n')
      const result = []
      const headers = lines[0].split(',')
      vm.parse_header = lines[0].split(',')
      lines[0].split(',').forEach(function (key) {
        vm.sortOrders[key] = 1
      })

      lines.map(function (line, indexLine) {
        if (indexLine < 1) return null // Jump header line

        const obj = {}
        const currentline = line.split(',')

        headers.map(function (header, indexHeader) {
          return (obj[header] = currentline[indexHeader])
        })

        return result.push(obj)
      })

      result.pop() // remove the last item because undefined values

      return result // JavaScript object
    },
    loadCSV(e) {
      const vm = this
      if (window.FileReader) {
        const reader = new FileReader()
        reader.readAsText(e.target.files[0])
        // Handle errors load
        reader.onload = function (event) {
          const csv = event.target.result
          vm.parse_csv = vm.csvJSON(csv)
        }
        reader.onerror = function (evt) {
          if (evt.target.error.name === 'NotReadableError') {
            alert("Canno't read file !")
          }
        }
      } else {
        alert('FileReader are not supported in this browser.')
      }
    },
  },
}
</script>
