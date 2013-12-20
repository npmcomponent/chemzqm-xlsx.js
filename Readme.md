
# xlsx.js

  converting the data in base64 XLSX files into JavaScript objects - and back

## Installation

  Install with [component(1)](http://component.io):

    $ component install chemzqm/xlsx.js

## Example

  Generate xlsx file

  ``` js
  var xlsx = require('xlsx.js');
  var $ = require('jquery');
  var data = [
    ['Person', 'Dogs'],
    ['a', 0],
    ['b', 1],
  ];
  $('#save').on('click', function() {
    var file = {
      worksheets: [[]], // worksheets has one empty worksheet (array)
      creator: 'John Smith', created: new Date('8/16/2012'),
      lastModifiedBy: 'Larry Jones', modified: new Date(),
      activeWorksheet: 0
    }, w = file.worksheets[0]; // cache current worksheet
    w.name = '测试';
    w.data = data;
    window.location = xlsx(file).href();
  });
  ```
  Parse xlsx file

  ``` js
  //test read xlsx file
  var xhr = new XMLHttpRequest();
  xhr.open('GET', 'download.xlsx', true);

  xhr.responseType = 'arraybuffer';

  xhr.onload = function(e) {
    if (this.status == 200) {
      var uInt8Array = new Uint8Array(this.response);
      var i = uInt8Array.length;
      var binaryString = new Array(i);
      while (i--)
      {
        binaryString[i] = String.fromCharCode(uInt8Array[i]);
      }
      var data = binaryString.join('');

      var base64 = window.btoa(data);
      console.log(xlsx(base64).worksheets[0]);
      //[
      //[[{value: 'Person'}], [{value: 'Dogs'}]],
      //[[{value: 'a'}], [{value: '0'}]],
      //[[{value: 'b'}], [{value: '1'}]]
      //]
    }
  };

  xhr.send();
  ```


## License

  MIT
