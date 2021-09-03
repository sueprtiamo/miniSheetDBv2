## miniSheetDB v2

Pustaka untuk [Google Apps Script (GAS)](https://www.google.com/script/start/) yang memberdayakan [Google Spreadsheet](https://docs.google.com/spreadsheets/?usp=mkt_sheets) menjadikan seolah-olah sebagai database aplikasi.

Ini adalah penerus dari versi lama [miniSheetDB v1](https://github.com/banghasan/minisheetdb) yang mana proyek yang awalnya berada pada repository pribadi [bangHasan](https://github.com/banghasan) sekarang dialihkan menjadi repository organisasi.

### Keuntungan

- 一般来说，语法与 `miniSheetDB v1` 没有太大区别
- 更容易管理电子表格
- 不仅适用于电报机器人，还可用于各种脚本需求
- 现在有更多方法、变量和灵活的数据管理选项
- 搜索结果中包含  “列 kolom”  和  “行 baris”  位置，后续处理不复杂
- 开源，安全查看，共同研究和开发。
- 类结构更整洁，易于使用、学习和/或派生

### Peringatan

`v2` 的结果与 `v1` 不兼容，尽管语法有很多相似之处。

> 始终检查日志方法结果以确保。

## Pemakaian

### ID

- 旧版: `MbEHC24_R5YUoHX8iCbUEAaZTb1melOAr`
- 新版编辑器: `1wMvpNwIL8fCMS7gN5XKPY7P-w4MmKT9yt_g2eXDGBtDErOIPq2vcNxIN`

### Start

```javascript
var ssid = 'idsheet';
var db = new miniSheetDB2.init(ssid);

function getAll() {
    let result = db.getAll();
    Logger.log(result);
}

function getKey() {
    let result = db.key('112233');
    Logger.log(result);
}
```

## Referensi

### Inisiasi

    init(id, tab = 0, options = {})

- `id` (`string`) 可以是工作表 ID 或 URL
- `tab` (`number` atau `sting`), 默认是索引为 0（第一个）的标签页。该值可以是工作表索引号或工作表名称。例如`'Sheet1'`
- `options` - 数据库上的`options` 操作选项。检查下面。

### Options

| **variable** | **tipe** | **default** | **keterangan**                                                                                                                                                                                    |
| ------------ | -------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `col_start`   | `number`   | 1           | sheet kolom dimulai                                                                                                                                                                                       |
| `col_length`  | `number`   | 2           | rentang sheet panjang kolom                                                                                                                                                                                       |
| `row_start`   | `number`   | 1           | baris dimulainya pemrosesan sheet                                                                                                                                                                                      |
| `json`         | `boolean`  | `false`       | Secara default (spreadsheet), hasil method `getAll` adalah bertipe Array. Maka, jika options `json` diisi `true` hasilnya akan diubah menjadi bertipe json. <br /><br />Konsekuensi nya adalah:<br /><br /> <ul><li>Key kosong diubah menjadi key bernilai `___`</li><li>Value yang dipakai adalah value yang ditemukan oleh key paling akhir</li></ul>  |

#### Contoh Pemakaian:

```javascript
var ssid = 'https://docs.google.com/spreadsheets/d/1B8JSBXqV0sIFZsuwDHQ8wOADFIAxgB7WDpJRh1JUei8/edit';
var db = new miniSheetDB2.init(ssid, 'Sheet1', {    
    col_length: 5,
    row_start: 2,
    json: true
});
```

#### Option bisa juga diset sesudah diinisiasi

Misalnya:

```javascript
db.col_length = 10;
```

### Method

Berikut list method, accessor, maupun field / tipe yang berada dalam class **miniSheetDB2**.

| **Method**  | **Params**               | **Keterangan**                                                                                                                                                                                                 |
| ----------- | ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`        | -                |获取/输入输入`id` |
| `ssid`        | -                 |获取正在使用的活动 ssid |                                                                                                                       
| `type`        | -                |获取活动的ssid类型，根据输入的ssid类型，结果是一个值为`http`或`id`的字符串|
| `url`         | -                |获取当前活动 url |
| `name`        | -                |获取当前使用的选项卡的工作表名称（标题） |
| `sheet`       | -                |表操作的主要指针。<br />检查[电子表格]中的方法 |
| `last_row`   | -                        |获取工作表行的最后一个位置 |
| `data_range` | - |在 A1Notation 中获取范围数据
| `range()`     | a1Notation, kolom, baris |获取指针范围以进行进一步处理。 <br />检查[范围]中的方法|
| `add(data)` | `array` 或 `data` |添加数据。 |
| `getValue()`  | a1Notation, kolom, baris |获取范围的内容 |
| `getValues()` | a1Notation, kolom, baris |批量获取范围的内容 |
| `setValue()`  | a1Notation, kolom, baris |填写一个范围的值 |
| `setValues()` | a1Notation, kolom, baris |批量填写一个范围的值|
| `getAll()`    | `boolean`                  |根据初始参数获取工作表的全部内容 |
| `key()`       | id (`string` atau `number`)                      |通过 id / key 搜索范围的内容 |
| `keys()`      | `[id1, id2, ...id]`        |通过 id/key 批量搜索范围的内容 |
| `search()`    | `String` atau `regexp`          |使用字符串或正则表达式按键搜索 |
| `searchAll()`    | `String` atau `regexp`          |基于key的多次搜索，如果找到则继续下一个字段 

## Penjelasan Detail

Penjelasan detail untuk beberapa method penting dan beberapa kasus, disertai berbagai contoh.

### add

添加数据。

```javascript
var db = new miniSheetDB2.init('a1b2c3');

// add with array type
var result = db.add(['kunci', 'isi']);
// or normal methode
var result = db.add('kunci', 'isi');
```

Contoh lain jika panjang kolom berbeda:

```javascript
var db = new miniSheetDB2.init('a1b2c3', 'Sheet1', { col_length: 3 });

// add with array type
var data = ['kunci', 'isi 1', 'isi 2'];
var result = db.add(data);
// or normal methode
var result = db.add('kunci', 'isi 1', 'isi 2');
```

Akan error jika panjang kolom tidak sesuai dengan data:

```javascript
var db = new miniSheetDB2.init('a1b2c3', 'Sheet1', { col_length: 3 });

// ERROR :
var result = db.add('kunci', 'isi 1', 'isi 2', 'isi 3', 'isi 4'); // len = 5
```


### key

mendapatkan data berdasarkan `id`.

`id` harus sama identik dengan key.

jika ditemukan, ada field `range` untuk pengolahan advance.

```javascript
// id = hasanudin
// id = 12345
var result = db.key(12345);  // ketemu
var result = db.key('hasanudin'); // ketemu
var result = db.key('hasan'); // tidak ketemu
```

### keys

mendapatkan data berdasarkan `id` secara massal (lebih dari satu)

```javascript
// penjelasan idem dengan method key
var result = db.keys([111, 222, 333]);
var result = db.keys(['hasan', 'amir']);
```

### search

Mendapatkan data berdasarkan `id` menggunakan reguler expression.

`id` bisa berupa `number`, `string` atau `regex`

Perbedaan jika menggunakan method `key` jika bertipe string: 

- method `search` data parsial tetap ditemukan
- method `key` harus bertipe eksak (sama persis).

jika ditemukan, ada field `range` untuk pengolahan advance.

Terdapat juga field `match` yang merupakan hasil regex.

```javascript
// id = Hasanudin
// id = ikhsan
var result = db.search('hasan'); // ketemu
var result = db.search(/^hasan$/); // tidak ketemu
var result = db.search(/san$/i); // ketemu
```

### searchAll

Serupa dengan method `search` diatas.

Contoh kasus, ingin menemukan semua kunci yang berawalan angka depan `71`

Hasil berupa array.

```javascript
// id key = 700, 711, 712, 811, 812
var result = db.searchAll(/^71/); // 711, 712
```

### getValue

Ref: [getValue](https://developers.google.com/apps-script/reference/spreadsheet/range#getvalue)

mendapatkan isi sebuah range

```javascript
var result = db.getValue('Sheet1!A1');
var result = db.getValue('A1');
var result = db.getValue(1,1);
```

### getValues

Ref: [getValues](https://developers.google.com/apps-script/reference/spreadsheet/range#getvalues)

mendapatkan isi range secara jamak

```javascript
var result = db.getValues('A1');
var result = db.getValues('Sheet1!A1');
var result = db.getValues('Sheet1!A1:C4');
var result = db.getValues(1,1);
var result = db.getValues(1,1,2);
var result = db.getValues(1,1,2,10);
```

### setValue
Ref: [setValue](https://developers.google.com/apps-script/reference/spreadsheet/range#setvaluevalue)

mengisi nilai suatu `range`, jika berhasil result berupa object [range].

```javascript
var result = db.setValue('rules!A1', 'Test');

db.setValue('A1', 'Test');
db.setValue(1, 1, 'Test');
```

### setValues

Ref: [setValues](https://developers.google.com/apps-script/reference/spreadsheet/range#setvaluesvalues)

Mengisi nilai beberapa range sekaligus, dengan value bertipe array.

Contoh beberapa variasi:

```javascript
var resulit = db.setValues('rules!A1', [['Test A1']]);

db.setValues('A10:A11', [['Baris 1'], ['Baris 2']]);
db.setValues('A10:B11', [['Baris 1', 'Samping 1'], ['Baris 2', 'Samping 2']]);

db.setValues(10, 1, [['Test A10']] );
db.setValues(10, 1, 2, [['Baris 1'], ['Baris 2']] );
db.setValues(10, 1, 2, 2, [['Baris 1', 'Samping 1'], ['Baris 2', 'Samping 2']] );
```

### getAll

Untuk mendapatkan semua data sheet aktif:

```javascript
// tipe array
// semua data tampil apa adanya
var resArray = db.getAll();

// untuk tipe JSON
// key kosong diubah ___
// jika key sama, data terakhir akan menimpa
var resJSON = db.getAll(true);
```


## sheet

### Bikin Baru

```javascript
db.app.insertSheet();
db.app.insertSheet(1); // sheet baru posisi ke-2
db.app.insertSheet('Sheet Baru');
db.app.insertSheet('Sheet Baru Paling Depan', 0);
```

### Hapus

Ref: [deleteRow](https://developers.google.com/apps-script/reference/spreadsheet/spreadsheet#deleterowrowposition)

Menghapus baris (row) :

```javascript
// hapus baris 2
db.sheet.deleteRow(2);

// hapus 5 baris sekaligus dimulai dari baris ke 2
db.sheet.deleteRow(2, 5);
```

Hapus baris pada posisi yang ditemukan

```javascript
// cari key
var hasil = db.key('id');
if (hasil) {
    // hapus baris ditempat key ditemukan
    db.sheet.deleteRow(hasil.row);
}
```


### Clear

Membersihkan range .

```javascript
// dibersihkan seluruhnya, isi dan format
db.range('A1:B1').clear();
// bersihkan isinya saja
db.range('A1:B1').clear({contentsOnly: true});

// membersihkan format saja
db.range('A1:B1').clearFormat();
// membersihkan isinya saja
db.range('A1:B1').clearContent();
```

### Data Range

Menghasilkan jangkuan data yang telah dipakai.

```javascript
vat dataRange = db.data_range; // misal: A1:E217
```

### Checkbox

Ref: [checkbox](https://developers.google.com/apps-script/reference/spreadsheet/range#insertcheckboxes)

```javascript
vat result = db.range('A1').insertCheckboxes();
vat result = db.range('A1').insertCheckboxes('yes');
```

### Style

```javascript
db.range('C1:D1').setFontColor("red");
db.range('A1:A2').setBackground("black");

var style = SpreadsheetApp.newTextStyle()
    .setFontSize(15)
    .setUnderline(true)
    .setFontColor("red")
    .build();
db.range('A1:B1').setTextStyle(style);
```

## Kontribusi

- Silakan di fork dan pull request
- Ada masalah atau saran pengembangan? silakan buat [issue baru](https://github.com/telegrambotindonesia/miniSheetDBv2/issues).

[range]: https://developers.google.com/apps-script/reference/spreadsheet/range
[spreadsheet]: https://developers.google.com/apps-script/reference/spreadsheet/spreadsheet
