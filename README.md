USB Forensics with Autopsy
<p align="center"> <b>Projeto prático de Forense Digital</b><br> Simulação de investigação em USB virtual com recuperação de arquivo apagado </p>
📌 Sobre o Projeto

Este projeto demonstra, de forma prática, como realizar uma análise forense em uma imagem de dispositivo USB contendo um arquivo apagado, utilizando ferramentas de forense digital no Kali Linux.

O objetivo é evidenciar que a exclusão lógica de arquivos não remove imediatamente os dados físicos do dispositivo, sendo possível sua recuperação por técnicas forenses.

🛠 Ferramentas Utilizadas

*Kali Linux

*Autopsy

*Sleuth Kit

*Terminal Linux

🎯 Objetivo

Criar um dispositivo USB virtual

Inserir e apagar arquivos

Analisar a imagem forense

Localizar e recuperar arquivos deletados

Validar integridade por hash SHA-256

⚙️ Metodologia
1️⃣ Criação da imagem (USB virtual)
```bash
dd if=/dev/zero of=usb.img bs=1M count=50
fdisk usb.img
```

Dentro do fdisk, digite:

```bash
n
p
1
Enter
Enter
w
```

Associar ao loop device:

```bash
sudo losetup -fP usb.img
```

Formatar a partição:

```bash
sudo mkfs.vfat /dev/loop0p1
```

Montar a partição:
```bash

sudo mkdir /mnt/usb
sudo mount /dev/loop0p1 /mnt/usb
````
2️⃣ Inserção de arquivos
```bash

echo "relatorio secreto" | sudo tee /mnt/usb/segredo.txt
echo "foto" | sudo tee /mnt/usb/foto.jpg
sync
````
3️⃣ Exclusão do arquivo (simulação)
```bash

sudo rm /mnt/usb/segredo.txt
sync
````
Desmontar e remover loop:
```bash

sudo umount /mnt/usb
sudo losetup -d /dev/loop0
````
4️⃣ Geração de hash (cadeia de custódia)
```bash

sha256sum usb.img > hash.txt
````
5️⃣ Análise no Autopsy
```bash

autopsy
````
Abrir no navegador:

http://localhost:9999

Fluxo no Autopsy:

Create New Case

Add Host

Add Image → Disk Image → usb.img

Clique em Analyze

Marque:

File Type

Deleted Files

Clique em Analyze novamente

Acesse:
File Analysis → All Deleted Files → segredo.txt

6️⃣ Recuperação do arquivo

No arquivo segredo.txt:

Clique em Export

Gere o hash do arquivo recuperado:
```bash

sha256sum segredo.txt
````
📸 Evidências

Foram coletados prints das seguintes telas:

Lista de arquivos deletados

Conteúdo do arquivo segredo.txt

Hash da imagem

Exportação do arquivo recuperado

(Armazenados na pasta prints/)

✅ Resultados

O arquivo segredo.txt foi identificado na lista de arquivos deletados e seu conteúdo foi visualizado e recuperado com sucesso.

🔐 Integridade

A integridade da evidência foi garantida por meio de hash SHA-256 da imagem analisada e do arquivo recuperado.

🧾 Conclusão

Este experimento demonstrou que a exclusão lógica de arquivos em sistemas FAT32 não remove imediatamente os dados do dispositivo, sendo possível recuperá-los por técnicas de forense digital.

📁 Estrutura do Repositório
usb-forensics-autopsy/
 ├ usb.img
 ├ hash.txt
 ├ README.md
 └ prints/
⚠️ Aviso

Este projeto tem fins educacionais e não deve ser utilizado para fins ilícitos.
