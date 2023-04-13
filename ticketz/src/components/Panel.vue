<template>

          <div class="panel">
            <div class="card-header panel-header">
              Edit Ticket
            </div>
            <div class="panel-body">
              <div class="form-group">
                <label for="title">Title</label>
                <input type="text" class="form-control" id="title" placeholder="Enter title">
              </div>
              <div class="form-group">
                <label for="operator">Operator</label>
                <select class="form-control" id="operator">
                  <option value="">Select operator</option>
                  <option value="operator1">Operator 1</option>
                  <option value="operator2">Operator 2</option>
                  <option value="operator3">Operator 3</option>
                </select>
              </div>
              <div class="form-group">
                <label for="status">Status</label>
                <select class="form-control" id="status">
                  <option value="">Select status</option>
                  <option value="new">New</option>
                  <option value="inprogress">In Progress</option>
                  <option value="resolved">Resolved</option>
                  <option value="closed">Closed</option>
                </select>
              </div>
              <button type="submit" class="btn btn-primary">Save changes</button>
            </div>
          </div>

</template>

<style scoped>
.panel {
    margin-top: 20px;
    width: 200px;
    background-color: #CFEBDF;
}
.card-header {
  border-radius: 10px 10px 0 0;
  background-color: #5F634F;
  color: #E2FADB;
}

.panel-header {
  background-color: #5F634F;
  border-radius: 10px 10px 0 0;
  color: white;
  padding: 10px;
  width: 200px;
  margin-left: -12px
}



.form-group {
    margin-bottom: 10px;
}

.form-group label {
    display: block;
    font-weight: bold;
    margin-bottom: 5px;
}

.form-control {
    width: 100%;
    padding: 5px;
    border: none;
    border-radius: 5px;
}

.btn {
    display: inline-block;
    padding: 6px 12px;
    margin-bottom: 0;
    font-size: 14px;
    font-weight: 400;
    line-height: 1.42857143;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    -ms-touch-action: manipulation;
    touch-action: manipulation;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    background-image: none;
    border: 1px solid transparent;
    border-radius: 4px;
}

button[type="submit"] {
  background-color: #5F634F;
  color: #E2FADB;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
}
</style>
<script>
export default {

}
</script>