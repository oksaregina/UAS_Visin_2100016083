# Ema Septian Hakim_2100016114
# Tabulasi Silang Jumlah Kematian di Indonesia berdasarkan Tahun dan Tipe dari Data Penyebab Kematian di Indonesia.

# Proses pembuatan tabulasi silang:

# 1. Buka Google Sheets
# 2. Pilih menu "Extensions" > "Apps Script" untuk membuka editor script di Google Sheets.
# 3. Salin dan tempelkan kode Apps Script ke dalam editor script.
Kodenya sebagai berikut:
function typeAndYear() {
  let ss = SpreadsheetApp.getActive();
  let sheet = ss.getSheetByName('Penyebab Kematian di Indonesia yang Dilaporkan - Clean.csv'); // Pastikan nama sheet yang disediakan dalam kode ("Penyebab Kematian di Indonesia yang Dilaporkan - Clean.csv") sesuai dengan nama sheet yang berisi data yang ingin diolah. Jika nama sheet tidak sesuai, gantilah dengan nama sheet yang benar.
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
  let newSheet = ss.insertSheet('Kematian Berdasarkan Tahun dan Tipe');
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

# 4. Klik ikon save (disket) atau tekan "Ctrl + S" untuk menyimpan script.
# 5. Jalankan fungsi typeAndYear() dengan cara menekan tombol "play" (ikon segitiga berjalan) di toolbar editor script. Ini akan memulai proses tabulasi silang.
# 6. Setelah script selesai dijalankan, akan muncul tab baru bernama "Kematian Berdasarkan Tahun dan Tipe" dengan hasil tabulasi silang di dalamnya.

Proses tabulasi silang dilakukan dengan mengumpulkan data dari sheet "Penyebab Kematian di Indonesia yang Dilaporkan - Clean.csv" dan menyusunnya dalam bentuk tabel 
yang menghitung total kematian berdasarkan tahun dan tipe penyebab kematian. Hasilnya akan ditampilkan dalam sheet baru "Kematian Berdasarkan Tahun dan Tipe".
