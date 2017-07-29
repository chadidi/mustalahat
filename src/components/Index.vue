<template>
  <div class="index container">
    <div class="modal fade" tabindex="-1" role="dialog" id="myModal">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
            <h4 class="modal-title">Modal title</h4>
          </div>
          <div class="modal-body">
            <div class="row">
              <div class="col-md-6">
                <div class="input-group">
                  <input type="text" class="form-control" placeholder="English name" v-model="tran.name">
                </div><!-- /input-group -->
              </div><!-- /.col-lg-6 -->
              <div class="col-md-6">
                <div class="input-group">
                  <input type="text" class="form-control" placeholder="Arabic Translation" v-model="tran.arname">
                </div><!-- /input-group -->
              </div><!-- /.col-lg-6 -->
            </div><!-- /.row -->
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" @click="saveTrans()">Save changes</button>
          </div>
        </div><!-- /.modal-content -->
      </div><!-- /.modal-dialog -->
    </div><!-- /.modal -->
    <h1>Welcome To Mustalahat</h1>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>English Name</th>
          <th>Arabic Name</th>
          <th>actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(tran, index) in paginate(trans)">
          <td>{{tran['.key']}}</td>
          <td>{{tran.name}}</td>
          <td>{{tran.arname}}</td>
          <td>
            <div class="btn-group">
              <button class="btn btn-sm btn-primary" @click="updateTrans(tran)"><span class="glyphicon glyphicon-pencil"></span></button>
              <button class="btn btn-sm btn-danger" @click="$parent.removeTrans(tran)"><span class="glyphicon glyphicon-remove"></span></button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="row">
      <ul class="pagination justify-content-center">
        <li class="page-item" v-if="isPrev">
          <a class="page-link" href="#" rel="prev" @click.prevent="pagination.current_page--">&laquo;</a>
        </li>
        <li class="page-item" v-if="isNext">
            <a class="page-link" href="#" rel="next" @click.prevent="pagination.current_page++">&raquo;</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'index',
  props: ['trans'],
  data () {
    return {
      tran: {},
      pagination: {
        per_page: 12,
        current_page: 1,
        total: 0
      }
    }
  },
  methods: {
    paginate (array) {
      let per_page = this.pagination.per_page
      let current_page = this.pagination.current_page
      --current_page; // because pages logically start with 1, but technically with 0
      return _.slice(array, current_page * per_page, (current_page + 1) * per_page);
    },
    updateTrans(tran) {
      this.tran = tran;
      $('#myModal').modal('show');
    },
    saveTrans(){
      this.$parent.saveTrans(this.tran);
      this.tran = {};
      $('#myModal').modal('hide');
    }
  },
  computed: {
    isPrev(){
      if(this.pagination.current_page>1)
        return true
      return false
    },
    isNext(){
      this.pagination.total = this.trans.length/this.pagination.per_page
      if (this.pagination.current_page<this.pagination.total)
        return true
      return false
    }
  }
}
</script>