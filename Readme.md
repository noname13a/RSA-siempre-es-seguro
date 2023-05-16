Herramientas:
RSACtfTool: https://github.com/RsaCtfTool/RsaCtfTool
rsatool: https://github.com/ius/rsatool

El primer paso para crear un par de llaves RSA vulnerable es ejecutar rsagen.py:

$ python3 rsagen.py

Una vez obtenidos n, d y e, se pasan a la siguiente herramienta, que genera la llave privada:

$ python3 rsatool.py -f PEM -o key.pem -n %N% -d %D% -e %E%

La llave pública se obtiene de la siguiente forma:

$ openssl rsa -in key.pem -pubout > key.pub

A la hora de generar el mensaje encriptado volvemos a hacer uso de openssl:

$ openssl pkeyutl -encrypt -inkey key.pub -pubin -in flag.txt -out flag.enc

Eliminamos key.pem, ya que nos interesa reventar RSA. Hay que comprobar que es vulnerable usando la herramienta RSACtfTool.
Primero extraemos la llave privada de la pública:

$ ./RsaCtfTool-master/RsaCtfTool.py --publickey key.pub --private

Bingo! Si es capaz de extraer la llave privada, solo hay que guardarla, y probar a descencriptar el mensaje, comprobando que obtenemos la flag:

$ openssl pkeyutl -decrypt -in flag.enc -out flag.txt -inkey key.pem

Los unicos archivos a los cuales se deberan tener acceso ser
