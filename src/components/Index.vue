<template>
  <div class="index container">
    <div id="table" class="table-editable">
      <span class="glyphicon glyphicon-plus" data-toggle="modal" data-target="#myModal"></span>
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
          <tr v-for="tran in trans">
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
    </div>
  </div>
</template>

<script>
export default {
  name: 'index',
  props: ['trans'],
  data () {
    return {
      tran: {}
    }
  },
  methods: {
    updateTrans(tran) {
      this.tran = tran;
      $('#myModal').modal('show');
    },
    saveTrans(){
      this.$parent.saveTrans(this.tran);
      this.tran = {};
      $('#myModal').modal('hide');
    }
  }
}
</script>