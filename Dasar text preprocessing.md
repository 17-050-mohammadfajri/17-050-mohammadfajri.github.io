Pada *natural language processing* (NLP), informasi yang akan digali berisi data-data yang strukturnya “sembarang” atau tidak terstruktur. Oleh karena itu, diperlukan proses pengubahan bentuk menjadi data yang terstruktur untuk kebutuhan lebih lanjut (*sentiment analysis*, *topic modelling*, dll).

> Text data needs to be cleaned and encoded to numerical values before giving them to machine learning models, this process of cleaning and encoding is called as **text preprocessing.**
>
> # Persiapan : Library yang dibutuhkan
>
> Salah satu keunggulan python adalah mendukung banyak *open-source library*. Ada banyak* library* python yang dapat digunakan untuk melakukan dan mengimplementasikan masalah dalam NLP.
>
> ## Natural Language Toolkit (NLTK)
>
> *Natural Language Toolkit* atau disingkat NLTK, adalah *libray *python untuk bekerja dengan permodelan teks. NLTK menyediakan alat yang baik mempersiapkan teks sebelum digunakan pada *machine learning* atau algoritma *deep learnin*g. Cara termudah untuk menginstall NLTK adalah menggunakan “pip” pada *command line*/*terminal*.
>
> ```
> pip install nltk
> ```
>
> Langkah pertama yang perlu anda lakukan setelah menginstall NLTK adalah mengunduh paket NLTK.
>
> ```
> import nltk
> nltk.download()
> ```

Dokumentasi lengkap dari NLTK dapat anda baca [disini](https://www.nltk.org/).

## Python Sastrawi (Stemming Bahasa Indonesia)

Python Sastrawi adalah pengembangan dari proyek [PHP Sastrawi](https://github.com/sastrawi/sastrawi). Python Sastrawi merupakan library sederhana yang dapat mengubah kata berimbuhan bahasa Indonesia menjadi bentuk dasarnya. Sastrawi juga dapat diinstal melalui “pip”.

```
pip install Sastrawi
```

Penggunaan Python Sastrawi sangat sederhana seperti baris kode dibawah ini :

```
# import StemmerFactory class
from Sastrawi.Stemmer.StemmerFactory import StemmerFactory# create stemmer
factory = StemmerFactory()
stemmer = factory.create_stemmer()# stemming process
sentence = 'Perekonomian Indonesia sedang dalam pertumbuhan yang membanggakan'output   = stemmer.stem(sentence)print(output)
# ekonomi indonesia sedang dalam tumbuh yang banggaprint(stemmer.stem('Mereka meniru-nirukannya'))
# mereka tiru
```

# 1. Case folding

*Case folding *adalah salah satu bentuk *text preprocessing* yang paling sederhana dan efektif meskipun sering diabaikan. Tujuan dari *case folding *untuk mengubah semua huruf dalam dokumen menjadi huruf kecil. Hanya huruf ‘a’ sampai ‘z’ yang diterima. Karakter selain huruf dihilangkan dan dianggap *delimiter*. Pada tahap ini tidak menggunakan *external library*apapun, kita bisa memanfaatkan modul yang tersedia di python.

Ada beberapa cara yang dapat digunakan dalam tahap *case folding*, anda dapat menggunakan beberapa atau menggunakan semuanya, tergantung pada tugas yang diberikan.

## Mengubah text menjadi lowercase

Salah satu contoh pentingnya penggunaan *lower case* adalah untuk mesin pencarian. Bayangkan anda sedang mencari dokumen yang mengandung “indonesia” namun tidak ada hasil yang muncul karena “indonesia” di indeks sebagai “INDONESIA”. Siapa yang harus kita salahkan? U.I. *designer*yang mengatur *user interface* atau *developer* yang mengatur sistem dalam indeks pencarian?

Contoh dibawah menunjukan bagaimana python mengubah teks menjadi *lowercase *:

```
kalimat = "Berikut ini adalah 5 negara dengan pendidikan terbaik di dunia adalah Korea Selatan, Jepang, Singapura, Hong Kong, dan Finlandia."lower_case = kalimat.lower()
print(lower_case)# output
# berikut ini adalah 5 negara dengan pendidikan terbaik di dunia adalah korea selatan, jepang, singapura, hong kong, dan finlandia.
```

## Menghapus angka

Hapuslah angka jika tidak relevan dengan apa yang akan anda analisa, contohnya seperti nomor rumah, nomor telepon, dll. *Regular expression*(*regex*) dapat digunakan untuk menghapus karakter angka. Python memiliki modul`re` untuk melakukan hal – hal yang berkaitan dengan *regex*.

Contoh dibawah menunjukan bagaimana python menghapus angka dalam sebuah kalimat :

```
import re # impor modul regular expressionkalimat = "Berikut ini adalah 5 negara dengan pendidikan terbaik di dunia adalah Korea Selatan, Jepang, Singapura, Hong Kong, dan Finlandia."
hasil = re.sub(r"\d+", "", kalimat)
print(hasil)# ouput
# Berikut ini adalah negara dengan pendidikan terbaik di dunia adalah Korea Selatan, Jepang, Singapura, Hong Kong, dan Finlandia.
```

## Menghapus tanda baca

Sama halnya dengan angka, tanda baca dalam kalimat tidak memiliki pengaruh pada *text preprocessing*. Menghapus tanda baca seperti [!”#$%&’()*+,-./:;<=>?@[\]^_`{|}~] dapat dilakukan di pyhton seperti dibawah ini :

```
kalimat = "Ini &adalah [contoh] kalimat? {dengan} tanda. baca?!!"
hasil = kalimat.translate(str.maketrans("","",string.punctuation))
print(hasil)# output
# Ini adalah contoh kalimat dengan tanda baca
```

## Menghapus whitepace (karakter kosong)

Untuk menghapus spasi di awal dan akhir, anda dapat menggunakan fungsi `strip()`pada pyhton. Perhatikan kode dibawah ini :

```
kalimat = " \t ini kalimat contoh\t "
hasil = kalimat.strip()
print(hasil)# output
# ini kalimat contoh
```

# 2. Tokenizing

*Tokenizing *adalah proses pemisahan teks menjadi potongan-potongan yang disebut sebagai token untuk kemudian di analisa. Kata, angka, simbol, tanda baca dan entitas penting lainnya dapat dianggap sebagai token. Didalam NLP, token diartikan sebagai “kata” meskipun *tokenize* juga dapat dilakukan pada paragraf maupun kalimat.

Fungsi `split()`pada pyhton dapat digunakan untuk memisahkan teks. Perhatikan contoh dibawah ini :

```
kalimat = "rumah idaman adalah rumah yang bersih"
pisah = kalimat.split()
print(pisah)# output
# ['rumah', 'idaman', 'adalah', 'rumah', 'yang', 'bersih']
```

Sayangnya, tahap *tokenizing* tidak sesederhana itu. Dari *output* kode diatas kita akan mengolah kata “rumah” dan “rumah” sebagai 2 entitas yang berbeda. Permasalahan ini tentunya akan mempengaruhi hasil dari analisa teks itu sendiri.

Untuk mengatasi masalah ini kita dapat menggunakan modul dari NLTK yang sudah diinstal sebelumnya.

## Tokenizing kata

Sebuah kalimat atau data dapat dipisah menjadi kata-kata dengan kelas `word_tokenize()` pada modul NLTK.

```
# impor word_tokenize dari modul nltk
from nltk.tokenize import word_tokenize 
 
kalimat = "Andi kerap melakukan transaksi rutin secara daring atau online."
 
tokens = nltk.tokenize.word_tokenize(kalimat)
print(tokens)# ouput 
# ['Andi', 'kerap', 'melakukan', 'transaksi', 'rutin', 'secara', 'daring', 'atau', 'online', '.']
```

Dari *output *kode diatas terdapat kemunculan tanda baca titik(.) dan koma (,) serta token “Andi” yang masih menggunakan huruf besar pada awal kata. Hal tersebut nantinya dapat menggangu proses perhitungan dalam penerapan algoritma. Jadi, sebaiknya teks telah melewati tahap *case folding *sebelum di *tokenize *agar menghasilkan hasil yang lebih konsisten.

Kita dapat menambahkan fungsi *case folding *untuk menghilangkan tanda baca dan mengubah *teks* ke dalam bentuk *lowercase*.

```
kalimat = kalimat.translate(str.maketrans('','',string.punctuation)).lower()# output
# ['andi', 'kerap', 'melakukan', 'transaksi', 'rutin', 'secara', 'daring', 'atau', 'online']
```

Kita juga bisa mendapatkan informasi frekuensi kemunculan setiap token dengan kelas `FreqDist()` yang sudah tersedia pada modul NLTK.

```
from nltk.tokenize import word_tokenize
from nltk.probability import FreqDistkalimat = "Andi kerap melakukan transaksi rutin secara daring atau online. Menurut Andi belanja online lebih praktis & murah."
kalimat = kalimat.translate(str.maketrans('','',string.punctuation)).lower()
 
tokens = nltk.tokenize.word_tokenize(kalimat)
kemunculan = nltk.FreqDist(tokens)
print(kemunculan.most_common())# output
# [('andi', 2), ('online', 2), ('kerap', 1), ('melakukan', 1), ('transaksi', 1), ('rutin', 1), ('secara', 1), ('daring', 1), ('atau', 1), ('menurut', 1), ('belanja', 1), ('lebih', 1), ('praktis', 1), ('murah', 1)]
```

Untuk menggambarkan ke dalam bentuk grafik, kita perlu menginstall library Matplotlib.

```
import matplotlib.pyplot as pltkemunculan.plot(30,cumulative=False)
plt.show()
```

![img](https://miro.medium.com/max/60/1*rohVFL9wTIOTkwnsSKrX1A.png?q=20)

Grafik frekuensi kemunculan kata pada dokumen

## Tokenizing kalimat

Prinsip yang sama dapat diterapkan untuk memisahkan kalimat pada paragraf. Anda dapat menggunkan kelas `sent_tokenize()` pada modul NLTK. Saya telah menambahkan kalimat pada contoh seperti dibawah ini :

```
# impor sent_tokenize dari modul nltk
from nltk.tokenize import sent_tokenizekalimat = "Andi kerap melakukan transaksi rutin secara daring atau online. Menurut Andi belanja online lebih praktis & murah."
 
tokens = nltk.tokenize.sent_tokenize(kalimat)
print(tokens)# ouput
# ['Andi kerap melakukan transaksi rutin secara daring atau online.', 'Menurut Andi belanja online lebih praktis & murah.']
```

# 3. Filtering (Stopword Removal)

*Filtering* adalah tahap mengambil kata-kata penting dari hasil token dengan menggunakan algoritma *stoplist *(membuang kata kurang penting) atau *wordlist* (menyimpan kata penting).

*Stopword* adalah kata umum yang biasanya muncul dalam jumlah besar dan dianggap tidak memiliki makna. Contoh *stopword* dalam bahasa Indonesia adalah “yang”, “dan”, “di”, “dari”, dll. Makna di balik penggunaan*stopword *yaitu dengan menghapus kata-kata yang memiliki informasi rendah dari sebuah teks, kita dapat fokus pada kata-kata penting sebagai gantinya.

Contoh penggunaan *filtering* dapat kita temukan pada konteks mesin pencarian. Jika permintaan pencarian anda adalah “apa itu pengertian manajemen?” tentunya anda ingin sistem pencarian fokus pada memunculkan dokumen dengan topik tentang “pengertian manajemen” di atas dokumen dengan topik “apa itu”. Hal ini dapat dilakukan dengan mencegah kata dari daftar *stopword* dianalisa.

## Filtering dengan NLTK

```
from nltk.tokenize import sent_tokenize, word_tokenize
from nltk.corpus import stopwords
 
kalimat = "Andi kerap melakukan transaksi rutin secara daring atau online. Menurut Andi belanja online lebih praktis & murah."
kalimat = kalimat.translate(str.maketrans('','',string.punctuation)).lower()
 
tokens = word_tokenize(kalimat)
listStopword =  set(stopwords.words('indonesian'))
 
removed = []
for t in tokens:
    if t not in listStopword:
        removed.append(t)
 
print(removed)# ouput
# ['andi', 'kerap', 'transaksi', 'rutin', 'daring', 'online', 'andi', 'belanja', 'online', 'praktis', 'murah']
```

## Filtering dengan sastrawi

Selain untuk *stemming, *library Sastrawi juga mendukung proses *filtering*. Kita dapat menggunakan `stopWordRemoverFactory` dari modul sastrawi.

Untuk melihat daftar *stopword* yang telah didefinisikan dalam library Sastrawi dapat menggunakan kode berikut :

```
from Sastrawi.StopWordRemover.StopWordRemoverFactory import StopWordRemoverFactoryfactory = StopWordRemoverFactory()
stopwords = factory.get_stop_words()
print(stopwords)
```

Kode diatas akan menampikan *stopword *yang tersedia di library Sastrawi. Prosos *filtering* pada Sastrawi dapat dilihat pada baris kode dibawah :

```
from Sastrawi.StopWordRemover.StopWordRemoverFactory import StopWordRemoverFactory
from nltk.tokenize import word_tokenizefactory = StopWordRemoverFactory()
stopword = factory.create_stop_word_remover()kalimat = "Andi kerap melakukan transaksi rutin secara daring atau online. Menurut Andi belanja online lebih praktis & murah."
kalimat = kalimat.translate(str.maketrans('','',string.punctuation)).lower()stop = stopword.remove(kalimat)
tokens = nltk.tokenize.word_tokenize(stop)
print(tokens)# output
# ['andi', 'kerap', 'transaksi', 'rutin', 'daring', 'online', 'andi', 'belanja', 'online', 'praktis', 'murah']
```

Kita dapat menambah atau mengurangi kata pada daftar *stopword* sesuai dengan kebutuhan analisa. Pada dasarnya daftar *stopword *pada library Sastrawi tersimpan di dalam list yang anda lihat [disini](https://github.com/har07/PySastrawi/blob/0ab8ce2a994679af63880f3bdd1bb23570ffc010/src/Sastrawi/StopWordRemover/StopWordRemoverFactory.py#L14). Jadi sebenarnya kita tinggal mengubah daftar pada list tersebut. Tetapi hal tersebut bisa menjadi permasalahan apabila pada suatu kasus kita diharuskan menambahkan *stopword* secara dinamis.

Library Sastrawi dapat mengatasi permasalahan tersebut, perhatikan kode dibawah ini :

```
from Sastrawi.StopWordRemover.StopWordRemoverFactory import StopWordRemoverFactory, StopWordRemover, ArrayDictionary
from nltk.tokenize import word_tokenize 
 

stop_factory = StopWordRemoverFactory().get_stop_words() #load defaul stopword
more_stopword = ['daring', 'online'] #menambahkan stopwordkalimat = "Andi kerap melakukan transaksi rutin secara daring atau online. Menurut Andi belanja online lebih praktis & murah."
kalimat = kalimat.translate(str.maketrans('','',string.punctuation)).lower()data = stop_factory + more_stopword #menggabungkan stopword
 
dictionary = ArrayDictionary(data)
str = StopWordRemover(dictionary)
tokens = nltk.tokenize.word_tokenize(str.remove(kalimat))
 
print(tokens)# output
# ['andi', 'kerap', 'transaksi', 'rutin', 'andi', 'belanja', 'praktis', 'murah']
```

Menurut 

[Jim Geovedi](https://medium.com/u/f8556bb2f8ea?source=post_page-----a4fa52608ffe--------------------------------)

 

 

stopwords 

# 4. Stemming

*Stemming* adalah proses menghilangkan [infleksi](https://m.belajarbahasa.id/artikel/dokumen/273-mengenal-infleksi-dan-derivasi-di-dalam-bahasa-2016-12-28-03-02) kata ke bentuk dasarnya, namun bentuk dasar tersebut tidak berarti sama dengan akar kata (*root word*). Misalnya kata “mendengarkan”, “dengarkan”, “didengarkan” akan ditransformasi menjadi kata “dengar”.

Idenya adalah ketika anda mencari dokumen “cara membuka lemari”, anda juga ingin melihat dokumen yang menyebutkan “cara terbuka lemari” atau “cara dibuka lemari” meskipun terdengar tidak enak. Tentunya anda ingin mencocokan semua variasi kata untuk memunculkan dokumen yang paling relevan.

## Stemming dengan NLTK (bahasa inggris)

Ada banyak algoritma yang digunakan untuk *stemming*. Salah satu algoritma yang tertua dan paling populer adalah [algoritma Porter](https://tartarus.org/martin/PorterStemmer/). Algoritma ini tersedia dalam modul NLTK melalui kelas`PorterStemmer()`.

```
from nltk.stem import PorterStemmer 
   
ps = PorterStemmer() 
  
kata = ["program", "programs", "programer", "programing", "programers"] 
  
for k in kata: 
    print(k, " : ", ps.stem(k))# ouput
# program  :  program
programs  :  program
programer  :  program
programing  :  program
programers  :  program
```

Selain Porter, NLTK juga mendukung algoritma [Lancester](http://www.comp.lancs.ac.uk/computing/research/stemming/Links/paice.htm), [WordNet Lemmatizer](https://wordnet.princeton.edu/), dan [SnowBall](http://snowball.tartarus.org/). Sayangnya proses *stemming* Bahasa Indonesia pada modul NLTK belum didukung.

## Stemming bahasa indonesia menggunakan Python Sastrawi

Proses *stemming* antara satu bahasa dengan bahasa yang lain tentu berbeda. Contohnya pada teks berbahasa inggris, proses yang diperlukan hanya proses menghilangkan [sufiks](https://id.wikipedia.org/wiki/Akhiran). Sedangkan pada teks berbahasa Indonesia semua kata imbuhan baik itu sufiks dan [prefiks](https://id.wikipedia.org/wiki/Awalan) juga dihilangkan.

Untuk melakukan *stemming* bahasa Indonesia kita dapat menggunakan library Python Sastrawi yang sudah kita siapkan di awal. *Library* Sastrawi menerapkan [Algoritma Nazief dan Adriani](https://dl.acm.org/citation.cfm?id=1316459) dalam melakukan *stemming*bahasa Indonesia.

```
from Sastrawi.Stemmer.StemmerFactory import StemmerFactoryfactory = StemmerFactory()
stemmer = factory.create_stemmer()
 
kalimat = "Andi kerap melakukan transaksi rutin secara daring atau online. Menurut Andi belanja online lebih praktis & murah."hasil = stemmer.stem(kalimat)print(hasil)# ouput
# andi kerap laku transaksi rutin cara daring atau online turut andi belanja online lebih praktis murah
```

# Apakah kita memerlukan semua tahapan pada text preprocessing*?*

Tidak ada aturan pasti yang membahas setiap tahapan pada *text preprocessing*. Tentu saja untuk memastikan hasil yang lebih baik dan konsisten semua tahapan harus dilakukan. Untuk memberi gambaran tentang apa yang minimal seharusnya dilakukan, saya telah menguraikan tahapan menjadi **harus dilakukan**, **sebaiknya dilakukan**, dan** tergantung tugas**. Perlu diingat, *less is more*, anda ingin menjaga pendekatan dengan seindah mungkin. Semakin banyak fitur atau tahapan yang anda tambahkan, semakin banyak pula lapisan yang anda harus kupas.

- **Harus dilakukan **meliputi *case folding* (dapat tergantung tugas dalam beberapa kasus)
- **Sebaiknya dilakukan **meliputi normalisasi sederhana — misalnya menstandarkan kata yang hampir sama
- **Tergantung tugas **meliputi normalisasi tingkat lanjut — misalnya mengatasi kata yang tidak biasa, s*topword removal *dan *stemming*

Jadi, untuk mengatasi tugas apapun minimal anda harus melakukan *case folding*. Selanjutnya dapat ditambahkan beberapa normalisasi dasar untuk mendapatkan hasil yang lebih konsisten dan secara bertahap menambahkan tahapan yang lainnya sesuai yang anda inginkan.

# Penutup

Dalam tulisan ini kita telah mengetahui langkah dasar dan praktis pada *text preprocessing *beserta *library* yang digunakan dalam python. Selanjutnya hasil dari *text preprocessing* dapat digunakan untuk analisa NLP yang lebih rumit, contohnya *machine translation*. Tidak semua kasus membutuhkan level *preprocessing* yang sama. Dalam beberapa kasus anda bisa menggunakan salah satu tahap dari *preprocessing *paling sederhana yaitu *case folding*. Namun semua tahap akan dibutuhkan apabila anda mempunyai *dataset* dengan level *noise* sangat tinggi.

Masih ada teknik yang bisa dilakukan pada *text preprocessing*, tetapi sesuai kata pengantar diatas, tulisan ini hanya mengulas langkah dasar dan praktis dalam *text preprocessing*.