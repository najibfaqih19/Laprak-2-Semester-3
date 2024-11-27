# Laprak-2-Semester-3
#Membuat Tabel 1
Tabel_1 <- data.frame(
  Nama = c("Andi", "Budi", "Citra", "Dimas", "Emma", "Farah", "Galih", "Hendra",
           "Indah", "Tika", "Rena", "Vina", "Rudi", "Taufik", "Avia", "Novia",
           "Wahyu", "Rizky", "Santi", "Cindy"),
  Kode_Produk = c("012A", "034B", "051F", "075H", "049E", "034B", "098N", "051F", "075H", "049E", "012A", "063G", "012A", "049E", "075H", "063G", "098N", "049E", "051F", "075H"),
  Jumlah_Produk = c(4,5,2,9,3,6,4,1, 3,8,5,10,6,3,8,2,4,10,1,4),
  stringsAsFactors = FALSE
)
print(Tabel_1)

#Membuat Tabel 2
Tabel_2 <- data.frame (
  Kode_Produk = c("012A", "034B", "049E", "051F", "063G", "075H", "098N"),
  Produk = c("Wajan", "Panci", "Gelas", "Rice Cooker", "Pisau", "Sapu", "Kain Pel"),
  Harga_Satuan = c(40000, 30000, 15000,175000, 10000, 20000,25000),
  stringsAsFactors = FALSE
)
print(Tabel_2)

#Membuat Tabel Gabungan
data1 <- merge(Tabel_1, Tabel_2, by="Kode_Produk", )
print(data1)

#Menambahkan total harga, harga diskon, dan total bayar
data1$Total_Harga <- data1$Harga_Satuan*data1$Jumlah_Produk
data1$Diskon <- ifelse (data1$Total_Harga <= 100000, 0.05*data1$Total_Harga, ifelse
  (data1$Total_Harga <= 500000, 0.1*data1$Total_Harga, 0.15*data1$Total_Harga))
data1$Total_Bayar <- data1$Total_Harga - data1$Diskon
print(data1)


#SOAL 1
data.bayar <- data1[data1$Total_Bayar >= 200000,]
print(data.bayar)

#Soal 2
data.wajan <-data1[data1$Produk == "Wajan" & data1$Jumlah_Produk >= 5,]

#Soal 3
install.packages("writexl")
install.packages("readxl")
install.packages("openxlsx")
library("openxlsx")
library("readxl")
library("writexl")
#Membuat file excel
write_xlsx(data1, "Data1.xlsx")
#Mencari letak file excel
getwd()
#Membaca file excel di R
data.penjualan = readxl::read_excel("Data1.xlsx")
data.penjualan
#Membuka file excel
buka.datapenjualan <- loadWorkbook("Data1.xlsx")
buka.datapenjualan
#Menambahkan sheet baru di excel
addWorksheet(buka.datapenjualan, sheetName = "Penjualan")
#Membuat data frame total pendapatan pada toko X hari itu
total_penjualan <- sum(data1$Total_Bayar)
total_pendapatan <- data.frame(Total_Penjualan = total_penjualan)
#Menambahkan data pada sheet baru di file excel
writeData(buka.datapenjualan, sheet = "Penjualan", total_pendapatan)
#Menyimpan perubahan pada excel
saveWorkbook(buka.datapenjualan, "Data1.xlsx", overwrite = TRUE)
