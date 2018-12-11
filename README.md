<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>jQuery Add / Remove Table Rows</title>
<style type="text/css">
    table{
        width: 100%;
        margin: 20px 0;
        border-collapse: collapse;
    }
    table, th, td{
        border: 1px solid #cdcdcd;
    }
    table th, table td{
        padding: 5px;
        text-align: left;
    }
</style>
<script src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
<script type="text/javascript">
    $(document).ready(function(){
  
          $(".add-row").click(function(){
            var name = $("#ProjectTitle").val();
            var email = $("#PID").val();
            var department = $("#form-control").val();
            var markup = "<tr><td><input type='checkbox' name='record'></td><td>" + name + "</td><td>" + email + "</td><td>" + department + "</td></tr>";
            $("table tbody").append(markup);
        });
        
        // Find and remove selected table rows
        $(".delete-row").click(function(){
            $("table tbody").find('input[name="record"]').each(function(){
                if($(this).is(":checked")){
                    $(this).parents("tr").remove();
                }
            });
        });
       
    });
//  function increment()
// { 
//   debugger;
//  var value = parseInt(document.getElementById('number').value , 10);
//  value = isNan(value)? 0:value;
//  value++;
//  document.getElementById('PID').value=value;
// }  
var url = "/_api/web/lists/getbytitle('Expense_Category')/items?$select=Department";

getItems(url).then(getItemsSuccess, getItemsFail);

function getItems(url){
    return $.ajax({
        url: _spPageContextInfo.webAbsoluteUrl + url,
        type: "GET",
        headers: {
            "accept": "application/json;odata=verbose",
        }
    });
}

function getItemsSuccess(data){
   if (data.d.results.length > 0) {
    var result =groupBy(data.d.results, 'Department'); // set value of result variable 
    $.each(result, function(index, item){
      // $('#form-control').append('<option value="' + value.Department + '">' + value.Department +'</option>');
         $('#form-control').append($('<option></option>').val(item).html(item));
        console.log(item); // access the choice field value
        
    });
   }
}

function getItemsFail(err){
    // error callback
    debugger;
} 
function groupBy(items, propertyName) {
    var finalresult = [];
    $.each(items, function(index, item) {
        if ($.inArray(item[propertyName], finalresult) == -1) {
            finalresult.push(item[propertyName]);
        }
    });
    return finalresult;
}


</script>
</head>
<body>
    <div id="MainForm">
      <tr>
      <td>Project Title</td>
      <td><input type="text" id="ProjectTitle" placeholder="Project Title"></td>
       <td>Project ID</td>
      <td><input type="text" id="PID" placeholder="PID"></td>
      <td>Select Department</td>
        <td><select id="form-control" style="width: 200px;">
          <option>Select Department</option>
          
      </select>
    </td>
    <td><input type="button" class="add-row" value="Add Row"></td>
    </tr>
    </div>
    <table>
        <thead>
            <tr>
                <th>Select</th>
                <th>Project Title</th>
                <th>Project ID</th>
                <th>Department</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><input type="checkbox" name="record"></td>
                
            </tr>
        </tbody>
    </table>
    <button type="button" class="delete-row">Delete Row</button>
</body> 
</html>
