# Topic Modeling

Topic modeling merupakan salah satu pendekatan pada Text Mining yang cukup handal dalam melakukan penemuan data-data teks yang tersembunyi dan menemukan hubungan antara teks yang satu dengan lainnya dari suatu corpus (Jelodar, et al., 2018).

Sederhananya yaitu mengelompokkan data teks berdasarkan suatu topik tertentu. Cara kerja topik modelling seperti clustering, dikatakan seperti clustering karena mengelompokkan dokumen berdasarkan kemiripannya. Topic Modelling termasuk unsupervised learning karena data yang digunakan tidak memiliki label.

> Konsep Topic Modeling terdiri dari entitas-entitas yaitu “kata”, “dokumen”, dan “corpora”. “Kata” dianggap sebagai unit dasar dari data diskrit dalam dokumen, didefinisikan sebagai item dari kosa kata yang diberi indeks untuk setiap kata unik pada dokumen. “Dokumen” adalah susunan N katakata. Sebuah corpus adalah kumpulan M dokumen dan corpora merupakan bentuk jamak dari corpus. Sementara “topic” adalah distribusi dari beberapa kosakata yang bersifat tetap. Secara sederhana, setiap dokumen dalam corpus mengandung proporsi tersendiri dari topik-topik yang dibahas sesuai kata-kata yang terkandung di dalamnya. Topic modeling telah menjadi bidang yang diminati oleh sebagian besar penulis dari bidang Text Mining, Natural Language Processing, dan Machine Learning (Verma & Gahier 2015).

Oke buddy, salah satu metode Topic Modeling adalah dengan menggunakan metode Latent Dirichlet Allocation (LDA). LDA pertama kali diperkenalkan oleh Blei, Ng dan Jordan pada tahun 2003, adalah salah satu metode paling populer dalam pemodelan topik. (Putra & Kusumawardani, 2017) mengatakan bahwa LDA dapat digunakan untuk meringkas, melakukan klasterisasi, menghubungkan maupun memproses data yang sangat besar karena LDA menghasilkan daftar topik yang diberi bobot untuk masing-masing dokumen. Topik yang muncul dari pengolahan data tersebut selanjutnya akan dilakukan uji koherensi topik, yaitu keterkaitan dari uraian probabilitas kata-kata yang ditemukan satu sama lain dalam menyusun suatu topik.

Topic Modeling sering digunakan pada data/volume teks yang besar, seperti pembicaraan di media sosial, ulasan pelanggan tentang hotel, film, dll. Namun, kali ini aku menggunakan data pertanyaan masyarakat seputar wisata kuliner yang ada di Yogyakarta (fyi aja data yang digunain yaitu data skripsi aku ehehe jadi ngga terlalu banyak). 

Ok Let’s begin!

> Hal pertama yang harus kita lakuin yaitu instalasi software ( aku gunain Anaconda3) trus modul atau package yang diperlukan jangan lupa.

```
import nltk
nltk.download('stopwords')
```

![img](https://miro.medium.com/max/60/1*nWo1VEfSxsjArfb8uFwozA.png?q=20)

```
import re, string, unicodedata  #modul regular expression
import nltk
from nltk import word_tokenize, sent_tokenize  #Paket ini membagi teks input menjadi kata-kata.,                                  from nltk.corpus import stopwords
```

Pada modul nltk terdapat stopwords yang akan digunakan. Oiya, sebelumnya stopwords bahasa indonesia aku tambah isinya sesuai dengan kebutuhan analisis. Misal kata, ‘jogja’, ‘yogya’, ‘tugu’, dll. Untuk melihat lokasi penyimpanan stopwords buka aja di: **C:\Users\asus\AppData\Roaming\nltk_data\corpora\stopwords**(tergantung tempat penyimpanan kalian ya, pokoknya di simpan pada**folder stopwords**)

Contohnya kaya gambar di bawah ini, tapi kalo aku namanya kuubah menjadi ‘stopword_id_kuliner’.

![img](https://miro.medium.com/max/60/1*tqg82GrG3jaNNMvgPQAiZA.png?q=20)

# **Data Pre-processing**

Preprocessing dilakukan untuk menghilangkan bagian atau teks yang tidak diperlukan sehingga mendapatkan data yang berkualitas untuk dieksekusi atau agar proses mining lebih akurat.

```
#preprocessingdef removeStopword(str):
    stop_words = set(stopwords.words('stopword_id_kuliner'))
    word_tokens = word_tokenize(str)
    filtered_sentence = [w for w in word_tokens if not w in stop_words]
    return ' '.join(filtered_sentence)#remove sentence which contains only one word
def removeSentence(str): 
    word = str.split()
    wordCount = len(word)
    if(wordCount<=1):
        str = ''
    
    return strdef cleaning(str):
    #remove non-ascii
    str = unicodedata.normalize('NFKD', str).encode('ascii', 'ignore').decode('utf-8', 'ignore')
    #remove URLs
    str = re.sub(r'(?i)\b((?:https?://|www\d{0,3}[.]|[a-z0-9.\-]+[.][a-z]{2,4}/)(?:[^\s()<>]+|\(([^\s()<>]+|(\([^\s()<>]+\)))*\))+(?:\(([^\s()<>]+|(\([^\s()<>]+\)))*\)|[^\s`!()\[\]{};:\'".,<>?«»“”‘’]))', '', str)
    #remove punctuations
    str = re.sub(r'[^\w]|_',' ',str)
    #remove digit from string
    str = re.sub("\S*\d\S*", "", str).strip()
    #remove digit or numbers
    str = re.sub(r"\b\d+\b", " ", str)
    #to lowercase
    str = str.lower()
    #Remove additional white spaces
    str = re.sub('[\s]+', ' ', str)
       
    return strdef preprocessing(str):
    str = removeSentence(str)
    str = cleaning(str)
    str = removeStopword(str)
    
    return str
```

Pada proses preprocessing di atas, kita mau ngilangin URLs jika ada, simbol-simbol, melakukan** Lower Casing** yaitu penyamaan case menjadi huruf kecil atau mengubah semua karakter pada teks menjadi huruf kecil. Karakter yang diproses hanya huruf ‘a’ hingga ‘z’.** Remove Punctuation,**punctuation diartikan sebagai tanda baca, berarti Remove Punctuation adalah sebagai menghapus tanda baca. Tahap ini Karakter selain huruf dihilangkan seperti koma, spasi, tanda tanya, dan lain sebagainya. **Stopword Removal **merupakan proses penghapusan kata yang sering muncul namun tidak memiliki makna. Berikut ini adalah contoh stopwords dalam Bahasa Indonesia: yang, juga, dari, dia, kami, kamu, aku, saya, ini, itu, atau, dan, dengan, adalah, yaitu, ke, tak, tidak, di, pada, jika, ya, dimana, pada, untuk, dan lain sebagainya. **Tokenizing **adalah proses memisah atau memecah kalimat menjadi potonganpotongan seperti kata-kata berdasarkan tiap kata yang menyusunnya

> kode di atas dites terlebih dahulu dengan memasukkan beberapa kalimat

```
#test the code
sentences = ["dimana lokasi kuliner jogja yang murah","alamat gudeg yu djum dimana sih, yang enak","s"]for st in sentences:
    r = preprocessing(st)
    print(r)
```

![img](https://miro.medium.com/max/60/1*Q4Lp6TrCokn4Rlx7j9-7Sw.png?q=20)

Kemudian lakukan preprocessing yang sesungguhnya dengan menggunakan data dalam bentuk excel seperti berikut.

```
#do preprocessing
import pandas as pd
import xlsxwriterfo = pd.read_excel('Data_Pertanyaan.xlsx', sheet_name='Sheet1') #read excel file
txt = fo['Pertanyaan']
workbook = xlsxwriter.Workbook('clean-data.xlsx')
worksheet = workbook.add_worksheet()row = 0
col = 0rowHeaders = ['text']
worksheet.write_row(row, col,  tuple(rowHeaders))
        
for t in txt:
    new_txt = preprocessing(t)
    rowValues = [new_txt]
    row += 1
    worksheet.write_row(row, col, tuple(rowValues))
    
workbook.close()
```

Dataku namanya Data_Pertanyaan.xlsx, yg terletak pada Sheet1 dan pada kolom yang diberi nama ‘Pertanyaan’.

> Jika berhasil, data cleaning otomatis akan tersimpan dengan nama data **‘clean-data.xlsx’**.

Berikut adalah contoh hasil data setelah di pre processing.

![img](https://miro.medium.com/max/60/1*RjRDhqQwe5vDjvvtX2oKng.png?q=20)

Nah buddy, untuk preprocessingnya sampai di sini dulu yaa… c u again in part 2.