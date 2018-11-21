```
function readFromTable() {
  var ss = SpreadsheetApp.getActive();
  var sheetDetails = ss.getSheetByName('Details');
  var sheetdata = ss.getSheetByName('Data');
  
  var host = sheetDetails.getRange("B1").getValue();
  var databaseName = sheetDetails.getRange("B2").getValue();
  var userName = sheetDetails.getRange("B3").getValue();
  var password = sheetDetails.getRange("B4").getValue();
  var port = sheetDetails.getRange("B5").getValue();
  var tableName = sheetDetails.getRange("B6").getValue();

  var url = 'jdbc:mysql://'+host+':'+port+'/'+databaseName;
  var sql = 'select * from' + tableName;
  try{
   var connection = Jdbc.getConnection(url, userName, password);
  
   var results = connection.createStatement().executeQuery(sql);
   var metaData = results.getMetaData();
   var columns = metaData.getColumnCount();
   
   var values = [];
   var value = [];
   var element = '';
   
   for(i=1; i<=columns; i++){
     element = metaData.getColumnLabel(i);
     value.push(element);
   
   }
   values.push(element);
   
   while(results.next()){
     values = [];
     for(i=1; i <= columns; i++){
       element = results.getString(i);
       value.push(element);
     }
     values.push(value);
   }
   }catch(err){
    SpreadsheetApp.getActive().toast('Your data has been refreshed.');
  }
  results.close();
  
  sheetdata.clear();
  sheetdata.getRange(1, 1, values.length, value.length).setValues(values);
  
}


```
