# massage
lagilagitest
#python 2.7.15

print "Hello, Dcoder!"
impor  urllib2
impor  urllib
impor  gzip
impor  os
impor  json
impor  sys
 waktu impor
mengimpor  StringIO

__author__  =  "X_P-2000"
__copyright__  =  "Hak Cipta 2020"
__credits__  = [ "X_P-2000" ]
__license__  =  "CC"
__version__  =  "1.0"
__maintainer__  =  "X_P-2000"
__email__  =  "solomonto2000@gmail.com"
__status__  =  "Produksi"

jika  len ( sys . argv ) <=  1 :
  print  "Penggunaan: \ n    python dumper.py [percakapan ID] [chunk_size (disarankan: 2000)] [{opsional} lokasi offset (default: 0)]"
  cetak  ""
  cetak  "python dumper.py 1075686392 2000 0"
  sys . keluar ()

error_timeout  =  30  # Ubah ini untuk mengubah batas waktu kesalahan (detik)
general_timeout  =  7  # Ubah ini untuk mengubah waktu tunggu setelah setiap permintaan (detik)
messages  = []
bicara  =  sys . argv [ 1 ]
offset  =  int ( sys . argv [ 3 ]) jika  len ( sys . argv ) > =  4  int lainnya  ( "0" )
messages_data  =  "lolno"
end_mark  =  " \" payload \ " : { \" end_of_history \ " "
limit  =  int ( sys . argv [ 2 ])
header  = { "origin" : "https://www.facebook.com" ,
"accept-encoding" : "gzip, deflate" ,
"accept-language" : "en-US, en; q = 0.8" ,
"cookie" : "your_cookie_value" ,
"pragma" : "no-cache" ,
"agen-pengguna" : "Mozilla / 5.0 (Macintosh; Intel Mac OS X 10_9_3) AppleWebKit / 537.36 (KHTML, seperti Gecko) Chrome / 37.0.2062.122 Safari / 537.36" ,
"tipe konten" : "application / x-www-form-urlencoded" ,
"accept" : "* / *" ,
"cache-control" : "no-cache" ,
"referer" : "https://www.facebook.com/messages/zuck" }

base_directory  =  "Pesan /"
direktori  =  base_directory  +  str ( bicara ) +  "/"
pretty_directory  =  base_directory  +  str ( bicara ) +  "/ Cukup /"

coba :
  os . makedirs ( direktori )
kecuali  OSError :
  pass  # sudah ada

coba :
  os . makedirs ( pretty_directory )
kecuali  OSError :
  pass  # sudah ada

sementara  end_mark  tidak ada  di  messages_data :

  data_text  = { "messages [thread_fbids] ["  +  str ( talk ) +  "] [offset]" : str ( offset ),
  "messages [thread_fbids] ["  +  str ( bicara ) +  "] [batas]" : str ( batas ),
  "klien" : "web_messenger" ,
  "__user" : "your_user_id" ,
  "__a" : "your __a" ,
  "__dyn" : "your __dyn" ,
  "__req" : "your __req" ,
  "fb_dtsg" : "your_fb_dtsg" ,
  "ttstamp" : "your_ttstamp" ,
  "__rev" : "your __rev" }
  data  =  urllib . urlencode ( data_text )
  url  =  "https://www.facebook.com/ajax/mercury/thread_info.php"
  
  cetak  "Mengambil pesan"  +  str ( offset ) +  "-"  +  str ( batas + offset ) +  "untuk ID percakapan"  +  str ( bicara )
  req  =  urllib2 . Permintaan ( url , data , tajuk )
  response  =  urllib2 . urlopen ( req )
  dikompresi  =  StringIO . StringIO ( respons . Read ())
  decompressedFile  =  gzip . GzipFile ( fileobj = dikompresi )
  
  
  outfile  =  terbuka ( direktori  +  str ( offset ) +  "-"  +  str ( batas + offset ) +  ".json" , 'w' )
  messages_data  =  decompressedFile . baca ()
  messages_data  =  messages_data [ 9 :]
  json_data  =  json . memuat ( messages_data )
  jika  json_data  adalah  tidak  ada  dan  json_data [ 'payload' ] adalah  tidak  ada :
    coba :
      messages  =  pesan  +  json_data [ 'payload' ] [ 'tindakan' ]
    kecuali  KeyError :
      sampaikan tidak  ada pesan lagi
  lain :
    cetak  "Kesalahan saat pengambilan. Mencoba lagi setelah"  +  str ( error_timeout ) +  "s"
    cetak  "Dump Data:"
    cetak  json_data
    waktu . sleep ( error_timeout )
    terus
  outfile . tulis ( messages_data )
  outfile . tutup ()  
  command  =  "python -mjson.tool"  +  direktori  +  str ( offset ) +  "-"  +  str ( batas + offset ) +  ".json>"  +  pretty_directory  +  str ( offset ) +  "-"  +  str ( batas + offset ) +  ".pretty.json"
  os . sistem ( perintah )
  offset  =  offset  +  batas
  waktu . tidur ( general_timeout )

finalfile  =  terbuka ( direktori  +  "complete.json" , 'wb' )
finalfile . tulis ( json . kesedihan ( pesan ))
finalfile . tutup ()
command  =  "python -mjson.tool"  +  direktori  +  "complete.json>"  +  pretty_directory  +  "complete.pretty.json"
os . sistem ( perintah )
