---
title: 友情链接
date: 2018-08-28 10:23:42
type: "links"

---
 <html>
 <head>
     <meta charset="utf-8">
     <title>html 简单的table样式</title>
     <style type="text/css">
     /* gridtable */
     table.gridtable {
         font-family: verdana,arial,sans-serif;
         font-size:11px;
         color:#333333;
         border-width: 1px;
         border-color: #666666;
         border-collapse: collapse;
     }
     table.gridtable th {
         border-width: 1px;
         padding: 8px;
         border-style: solid;
         border-color: #666666;
         background-color: #dedede;
     }
     table.gridtable td {
         border-width: 1px;
         padding: 8px;
         border-style: solid;
         border-color: #666666;
         background-color: #ffffff;
     }
     /* imagetable */
     table.imagetable {
         font-family: verdana,arial,sans-serif;
         font-size:11px;
         color:#333333;
         border-width: 1px;
         border-color: #999999;
         border-collapse: collapse;
     }
     table.imagetable th {
         background:#b5cfd2 url('cell-blue.jpg');
         border-width: 1px;
         padding: 8px;
         border-style: solid;
         border-color: #999999;
     }
     table.imagetable td {
         background:#dcddc0 url('cell-grey.jpg');
         border-width: 1px;
         padding: 8px;
         border-style: solid;
         border-color: #999999;
     }
     /* /imagetable */
     /* altrowstable */
     table.altrowstable {
         font-family: verdana,arial,sans-serif;
         font-size:11px;
         color:#333333;
         border-width: 1px;
         border-color: #a9c6c9;
         border-collapse: collapse;
     }
     table.altrowstable th {
         border-width: 1px;
         padding: 8px;
         border-style: solid;
         border-color: #a9c6c9;
     }
     table.altrowstable td {
         border-width: 1px;
         padding: 8px;
         border-style: solid;
         border-color: #a9c6c9;
     }
     .oddrowcolor{
         background-color:#d4e3e5;
     }
     .evenrowcolor{
         background-color:#c3dde0;
     }
     /* /altrowstable */
     /* hovertable */
     table.hovertable {
         font-family: verdana,arial,sans-serif;
         font-size:11px;
         color:#333333;
         border-width: 1px;
         border-color: #999999;
         border-collapse: collapse;
     }
     table.hovertable th {
         background-color:#c3dde0;
         border-width: 1px;
         padding: 8px;
         border-style: solid;
         border-color: #a9c6c9;
     }
     table.hovertable tr {
         background-color:#d4e3e5;
     }
     table.hovertable td {
         border-width: 1px;
         padding: 8px;
         border-style: solid;
         border-color: #a9c6c9;
     }
     /* /hovertable */
 
     </style>
 </head>
 <body>
<table class="imagetable">
     <tr>
         <th>Info Header 1</th>
         <th>Info Header 2</th>
         <th>Info Header 3</th>
     </tr>
     <tr>
         <td>Text 1A</td><td>Text 1B</td><td>Text 1C</td>
     </tr>
     <tr>
         <td>Text 2A</td><td>Text 2B</td><td>Text 2C</td>
     </tr>
 </table>
 <table class="hovertable">
     <tr>
         <th>Info Header 1</th><th>Info Header 2</th><th>Info Header 3</th>
     </tr>
     <tr onmouseover="this.style.backgroundColor='#ffff66';">
         <td>Item 1A</td><td>Item 1B</td><td>Item 1C</td>
     </tr>
     <tr onmouseover="this.style.backgroundColor='#ffff66';" >
         <td>Item 2A</td><td>Item 2B</td><td>Item 2C</td>
     </tr>
     <tr onmouseover="this.style.backgroundColor='#ffff66';">
         <td>Item 3A</td><td>Item 3B</td><td>Item 3C</td>
     </tr>
     <tr onmouseover="this.style.backgroundColor='#ffff66';" >
         <td>Item 4A</td><td>Item 4B</td><td>Item 4C</td>
     </tr>
     <tr onmouseover="this.style.backgroundColor='#ffff66';">
         <td>Item 5A</td><td>Item 5B</td><td>Item 5C</td>
     </tr>
 </table>
 
 <script type="text/javascript">
     function altRows(id){
         if(document.getElementsByTagName){
 
             var table = document.getElementById(id);
             var rows = table.getElementsByTagName("tr");
 
             for(i = 0; i < rows.length; i++){
                 if(i % 2 == 0){
                     rows[i].className = "evenrowcolor";
                 }else{
                     rows[i].className = "oddrowcolor";
                 }
             }
         }
     }
 
     window.onload=function(){
         altRows('alternatecolor');
     }
 </script>
 </body>
 </html>