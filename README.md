# usb-forensics-autopsy
Projeto prático de forense digital simulando a análise de um dispositivo USB virtual, com identificação e recuperação de arquivo apagado utilizando Autopsy.

Projeto: Forense Digital em USB Virtual com Autopsy
📌 Objetivo

Simular uma investigação forense em um dispositivo USB virtual contendo um arquivo apagado e demonstrar sua recuperação utilizando técnicas de forense digital.

🛠 Ferramentas Utilizadas

Kali Linux

Autopsy

Sleuth Kit

Terminal Linux

⚙️ Metodologia
1️⃣ Criação da imagem (USB virtual)
dd if=/dev/zero of=usb.img bs=1M count=50
fdisk usb.img

Dentro do fdisk:

n
p
1
Enter
Enter
w

Associar ao loop device:

sudo losetup -fP usb.img

Formatar a partição:

sudo mkfs.vfat /dev/loop0p1

Montar a partição:

sudo mkdir /mnt/usb
sudo mount /dev/loop0p1 /mnt/usb
2️⃣ Inserção de arquivos
echo "relatorio secreto" | sudo tee /mnt/usb/segredo.txt
echo "foto" | sudo tee /mnt/usb/foto.jpg
sync
3️⃣ Exclusão do arquivo (simulação)
sudo rm /mnt/usb/segredo.txt
sync

Desmontar e remover loop:

sudo umount /mnt/usb
sudo losetup -d /dev/loop0
4️⃣ Geração de hash (cadeia de custódia)
sha256sum usb.img > hash.txt
5️⃣ Análise no Autopsy
autopsy

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

Vá em:
File Analysis → All Deleted Files → segredo.txt

6️⃣ Recuperação do arquivo

No arquivo segredo.txt:

Clique em Export para recuperá-lo

Gere o hash do arquivo recuperado:

sha256sum segredo.txt
📸 Evidências

Foram coletados prints das seguintes telas:

Lista de arquivos deletados

Conteúdo do arquivo segredo.txt

Hash da imagem

Exportação do arquivo recuperado

(Adicionar na pasta prints/)

✅ Resultados

O arquivo segredo.txt foi localizado na lista de arquivos deletados e seu conteúdo pôde ser visualizado e recuperado com sucesso.

🔐 Integridade

A integridade da evidência foi garantida por meio da geração de hash SHA-256 da imagem analisada e do arquivo recuperado.

🧾 Conclusão

A análise demonstrou que a exclusão lógica de arquivos em sistemas de arquivos FAT32 não remove imediatamente os dados do dispositivo, permitindo sua recuperação por técnicas de forense digital.
