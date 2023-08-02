# Proses pembuatan tabulasi silang data tentang penyebab dan jumlah kematian per-tahun di Indonesia.
## Berikut adalah penjelasan prosesnya:

1. Buka Google Sheets menggunakan link yang diberikan [Data Kematian di Indonesia](https://www.kaggle.com/datasets/hendratno/cause-of-death-in-indonesia).

2. Klik pada menu "Ekstensi" dan pilih "Apps Script". Ini akan membuka editor skrip di Google Sheets.

3. Salin dan tempelkan kode berikut ke dalam editor skrip. Ini adalah fungsi JavaScript yang disebut `typeAndYear()`, yang bertugas membuat tabulasi silang data berdasarkan tipe dan jumlah kematian per tahun.

```javascript
function typeAndYear() {
  let ss = SpreadsheetApp.getActive();
  let sheet = ss.getSheetByName('Penyebab Kematian di Indonesia yang Dilaporkan - Clean');
  let numberRows = sheet.getDataRange().getNumRows();
  let numberCols = sheet.getLastColumn();

  let data = sheet.getRange(2, 1, numberRows - 1, numberCols).getValues();

  // Get unique years and types
  let years = [];
  let types = [];
  for (let i = 0; i < data.length; i++) {
    let year = data[i][2];
    let type = data[i][1];
    if (years.indexOf(year) == -1) years.push(year);
    if (types.indexOf(type) == -1) types.push(type);
  }

  // Sort years in ascending order
  years.sort((a, b) => a - b);

  // Create new sheet
  let newSheet = ss.insertSheet('Kematian di Indonesia Berdasarkan Tahun dan Tipe ');
  newSheet.getRange('A1').setValue('Type');
  for (let i = 0; i < years.length; i++) {
    newSheet.getRange(1, i + 2).setValue(years[i]);
  }

  // Calculate totals and add to sheet
  for (let i = 0; i < types.length; i++) {
    let type = types[i];
    newSheet.getRange(i + 2, 1).setValue(type);
    for (let j = 0; j < years.length; j++) {
      let year = years[j];
      let totalDeaths = 0;
      for (let k = 0; k < data.length; k++) {
        if (data[k][2] == year && data[k][1] == type) {
          totalDeaths += Number(data[k][4]);
        }
      }
      newSheet.getRange(i + 2, j + 2).setValue(totalDeaths);
    }
  }
}
```

4. Setelah memastikan tidak ada kesalahan, simpan skrip dengan menekan "Ctrl + S".

5. Klik tombol "jalankan" untuk menjalankan fungsi dan tunggu proses eksekusi fungsi selesai. Setelah selesai, akan ada lembar kerja baru dengan nama "Kematian di Indonesia Berdasarkan Tahun dan Tipe" terbentuk.

6. Lembar kerja baru tersebut akan berisi tabulasi silang data tentang penyebab dan jumlah kematian per tahun di Indonesia. Baris pertama akan berisi tipe-tipe penyebab kematian, sedangkan kolom pertama akan berisi tahun-tahun unik di mana laporan kematian dilaporkan.

7. Berikut adalah hasil tabulasi silang data:
```javascript
Type	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020	2021	2022
Bencana Alam	0	0	0	0	166698	1973	6960	562	262	1447	1306	172	174	0	0	215	442	169	3739	352	236	583	0
Bencana Non Alam dan Penyakit	339	324	435	633	54046	121063	120104	88713	106035	31661	37922	1967	1595	1145	1199	2096	2156	875	1212	14338	37823	138519	12876
Bencana Sosial	0	0	0	0	0	0	1	0	1	16	33	34	65	0	0	45	26	0	25	7	4	4	0
```
