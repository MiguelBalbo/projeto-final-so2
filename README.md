# Projeto Final - Capítulos 6, 7 e 9

## RELATÓRIO DE PRÁTICAS E CÓDIGOS

**Nome do Aluno:** Miguel Balbo Victor
**Turno:** Noite
**Data do Último Commit:** 28/11/2025

---

## ATIVIDADE 1: Relatório das Práticas de Aula

Esta seção documenta a execução das práticas de administração de sistemas realizadas em
sala, conforme solicitado no final de cada capítulo do livro-texto.
**Instrução:** Para cada prática, forneça um breve resumo do que foi feito e cole a saída de
texto dos comandos de validação solicitados. **Não use imagens (printscreens)**.

### Capítulo 6: Práticas de Discos e Montagem

#### Prática 8b65b431 01 (Livro-Texto p. 171)

- **Resumo da Prática:**

1.  O primeiro passo para a realização desse exercício é a criação de um novo disco. Para isso no software UTM (Software de virtualização para Mac OS e IOS), tive que abrir as configurações da máquina virtual, e em drives, selecionei a opção _New…_, e Em seguida, selecionei o tamanho e o tipo de drive (no caso, IDE mesmo), e selecionei a opção _create._

2.  O disco foi criado, mas ele está sem partições, ou seja, está inutilizável. Para corrigir isso, utilizei o comando _fdisk,_ aliada com as opções _n_ para novo disco. No setup da partição, eu utilizei das configurações padrão, então apenas apertei enter. Por fim, apertei _w_ para salvar as alterações no disco

3.  Após a partição criada, eu utilizei do comando _mkfs_ para formatar a partição para o formato ext4

```bash
sudo mkfs.ext4 /dev/sdb1 #formata partição
```

4.  Depois de tudo isso, utilizei essa sequência de comandos abaixo para criar a pasta e obter o ID da partição.

```bash
cd / #vai pra raiz
sudo mkdir backup #cria a pasta backup
sudo blkid #exibe id das partições
```

Com o _id_ obtido, o próximo passo é editar o arquivo de montagem, o _fstab_

```bash
sudo nano /etc/fstab #edite arquivo de montagem
```

No arquivo _fstab_, adicionei a seguinte linha ao final para garantir a montagem correta do disco. Após isso, saí do arquivo utilizando as teclas _ctrl + x,_ y \**para confirmar o salvamento e *enter\* para salvar no mesmo arquivo

```bash
UUID=2a37d785-7b85-4e3d-bd81-71b519d7cce3  /backup  ext4  defaults  0  2
#saida do arquivo com ctrl + x -> y pra sair e enter para selecionar nome do arq
```

Fora do arquivo, executei os seguintes comandos para realizar a montagem e a reinicialização da máquina

```bash
sudo mount -a
sudo reboot
```

Por fim, executei o comando de verificação, e obtive resultado de 100% de acerto

- **Evidência de Validação:**

```bash
# Saída do comando 'cat /etc/fstab'
    # /etc/fstab: static file system information.
    #
    # Use 'blkid' to print the universally unique identifier for a
    # device; this may be used with UUID= as a more robust way to name devices
    # that works even if disks are added and removed. See fstab(5).
    #
    # systemd generates mount units based on this file, see systemd.mount(5).
    # Please run 'systemctl daemon-reload' after making changes here.
    #
    # <file system> <mount point>   <type>  <options>       <dump>  <pass>
    # / was on /dev/sda2 during installation
    UUID=64b09ac5-2c50-474a-9584-c3e1ece9782c /               ext4    errors=remount-ro 0       1
    # /boot/efi was on /dev/sda1 during installation
    UUID=CA3D-6052  /boot/efi       vfat    umask=0077      0       1
    # swap was on /dev/sda3 during installation
    UUID=62e2f137-8d5d-4696-aea3-2e87955d06ae none            swap    sw              0       0
    /dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0
    UUID=2a37d785-7b85-4e3d-bd81-71b519d7cce3  /backup  ext4  defaults  0  0
# Saída do comando 'df -h'
    Filesystem      Size  Used Avail Use% Mounted on
    udev            958M     0  958M   0% /dev
    tmpfs           197M  628K  196M   1% /run
    /dev/sda2        15G  2.0G   12G  15% /
    tmpfs           983M     0  983M   0% /dev/shm
    tmpfs           5.0M     0  5.0M   0% /run/lock
    /dev/sdb1       2.0G   24K  1.9G   1% /backup
    /dev/sda1       511M  5.9M  506M   2% /boot/efi
    tmpfs           197M     0  197M   0% /run/user/1000
```

#### Prática 8b65b431 02 (Livro-Texto p. 172)

- **Resumo da Prática:**

1.  Como solicitado, realizei o download da imagem _iso_ disponibilizada no link

2.  No _UTM_, para configurar o disco, eu fui nas configurações da VM, e nos _IDE Drives_, selecionei o referente ao CD (ele possui o _Image Type_ de _CD/DVD(ISO) Image_, assim podendo ser facilmente identificado. Em _Path_, selecionei o caminho do arquivo _Iso_ no meu computador e salvei

3.  Para a criação do diretório, utilizei o comando

```bash
sudo mkdir -p /home/userlinux/cdrom #o -p cria no caminho
```

4.  Para a montagem do disco no caminho especificado, utilizei o comando

```bash
sudo mount /dev/sr0 /home/userlinux/cdrom
```

Por ser um disco, recebi um aviso de que o diretório é apenas leitura
Por fim, executei o comando de validação, recebendo um sucesso de 100%

- **Evidência de Validação:**

```bash
# Saída do comando 'df -h'
    Filesystem      Size  Used Avail Use% Mounted on
    udev            958M     0  958M   0% /dev
    tmpfs           197M  632K  196M   1% /run
    /dev/sda2        15G  2.0G   12G  15% /
    tmpfs           983M     0  983M   0% /dev/shm
    tmpfs           5.0M     0  5.0M   0% /run/lock
    /dev/sdb1       2.0G   24K  1.9G   1% /backup
    /dev/sda1       511M  5.9M  506M   2% /boot/efi
    tmpfs           197M     0  197M   0% /run/user/1000
    /dev/sr0        364K  364K     0 100% /home/userlinux/cdrom
# Saída do comando 'cat /home/usuario/cdrom/arquivo.txt'
    aiedonline
```

### Capítulo 7: Práticas de Processos

#### Prática prc0001 01 (Livro-Texto p. 233)

- **Resumo da Prática:** Para continuar a configuração do ambiente, segui uma sequência de passos diretamente pelo terminal, executando cada comando de forma organizada:
  Acessei o diretório inicial do meu usuário:
  O primeiro comando que utilizei foi:

```bash
cd ~/
```

Esse comando garante que eu volte para o meu home directory, facilitando a execução dos próximos passos em um local previsível.
Gerei a localidade en_US.UTF-8 no sistema:
Em seguida, executei o comando responsável por gerar a localidade en_US.UTF-8, necessária para aplicações que dependem desse padrão de idioma e codificação:

```bash
sudo locale-gen "en_US.UTF-8"
```

Ao executar esse comando, recebi o seguinte retorno, comunicando que o arquivo locales está sendo gerado:

```bash
    Generating locales (this might take a while)...
    en_US.UTF-8... done
    Generation complete.
```

Iniciei uma sessão de gravação de terminal com o comando script:
Para registrar tudo o que acontecia na sessão, executei:

```bash
script
```

Esse comando inicia o registro de toda a saída exibida no terminal, criando um arquivo chamado typescript no diretório atual.
Listei os processos ativos e filtrei apenas aqueles relacionados a Python:
Após iniciar o registro, verifiquei os processos em execução utilizando ps e, em seguida, filtrei para exibir apenas processos contendo a palavra “python”. Usei:

```bash
ps aux | grep python
```

Assim, pude visualizar claramente quais processos Python estavam ativos no sistema naquele momento.
Finalizei a sessão de gravação e encerrei o terminal:
Com todas as verificações concluídas, finalizei a sessão do comando script digitando:

```bash
exit
```

Esse comando encerrou a gravação e também fechou a sessão do terminal.

- **Evidência de Validação:**

```bash
# Saída do comando 'cat /home/usuario/typescript' (após filtrar por 'python')
    Script started on 2025-11-14 21:47:08-05:00 [TERM="xterm-256color" TTY="/dev/pts/0" COLUMNS="80" LINES="24"]
    userlinux@debian:~$ ps aux | grep python
    userlin+     648 25.0  0.1   6340  2164 pts/1    S+   21:47   0:00 grep python
    userlinux@debian:~$ exit
    exit

    Script done on 2025-11-14 21:47:22-05:00 [COMMAND_EXIT_CODE="0"]
```

### Capítulo 9: Práticas de Redes

#### Prática 0002 checkpoint03 (Livro-Texto p. 286)

- **Resumo da Prática:** Configurei o Adaptador de Rede do UTM — o software de virtualização que estou utilizando — para operar em modo NAT, garantindo que a máquina virtual tenha acesso à rede externa por meio do host. Em seguida, iniciei a VM normalmente para aplicar as configurações iniciais do ambiente.
  Após o sistema carregar, acessei o arquivo /etc/network/interfaces, responsável pela definição das interfaces e parâmetros de rede no Debian. Dentro dele, localizei a configuração referente ao adaptador enp0s1, que é a interface utilizada pela minha máquina virtual, e então ajustei manualmente seus parâmetros de rede. Defini:
- Endereço IP estático: 10.0.2.3
- Máscara de rede: /24 (equivalente a 255.255.255.0)
- Gateway padrão: 10.0.2.2
- Servidor DNS: 8.8.8.8
  E comentei as linhas anteriores referentes ao adaptador enp0s1, para não causar conflitos de configuração.
  Essas configurações foram inseridas diretamente no arquivo para garantir que a interface subisse automaticamente com esses valores a cada inicialização do sistema.
  Após finalizar a edição, salvei as alterações e, para garantir que tudo fosse aplicado corretamente, reiniciei a máquina virtual. Com isso, as novas configurações de rede passaram a valer e a VM pôde se comunicar adequadamente com a rede por meio do modo NAT configurado no UTM.
- **Evidência de Validação:**

```bash
# Saída do comando 'ip address show enp0s3'
    2: enp0s1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether ea:d8:ac:44:5f:36 brd ff:ff:ff:ff:ff:ff
        inet 10.0.2.3/24 brd 10.0.2.255 scope global enp0s1
        valid_lft 3002sec preferred_lft 3002sec
        inet6 fd2c:d5a5:bb17:fbfe:e8d8:acff:fe44:5f36/64 scope global mngtmpaddr
        valid_lft 2591971sec preferred_lft 604771sec
        inet6 fe80::e8d8:acff:fe44:5f36/64 scope link
        valid_lft forever preferred_lft forever
# Saída do comando 'ip route'
    default via 10.0.2.2 dev enp0s1
    10.0.2.0/24 dev enp0s1 proto kernel scope link src 10.0.2.3
# Saída do comando 'cat /etc/network/interfaces'
    # This file describes the network interfaces available on your system
    # and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # The primary network interface
    #allow-hotplug enp0s1
    #iface enp0s1 inet dhcp
    # This is an autoconfigured IPv6 interface
    #iface enp0s1 inet6 auto
    auto eпp0s1
        iface enp0s1 inet static
        address 10.0.2.3
        netmask 255.255.255.0
        gateway 10.0.2.2
        dns-nameservers 8.8.8.8
```

#### Prática 0002 checkpoint04 (Livro-Texto p. 287)

- **Resumo da Prática:** O primeiro passo foi editar o arquivo /etc/network/interfaces, removendo todas as configurações de IP que estavam definidas manualmente para a interface enp0s1. Isso incluiu os seguintes campos:
  address
  netmask
  network
  broadcast
  gateway
  dns-nameservers
  Ao apagar esses parâmetros, deixei a interface configurada apenas com a diretiva padrão de Auto DHCP, permitindo que o sistema obtivesse automaticamente todas as informações de rede diretamente do servidor DHCP fornecido pelo modo NAT do UTM. Após salvar as alterações, reiniciei a máquina para garantir que a nova configuração entrasse em vigor e que a interface subisse corretamente.
  Com a máquina já reiniciada e a interface limpa, realizei uma nova configuração de rede, dessa vez manualmente via comandos ip, definindo os parâmetros desejados para a interface enp0s1. Os comandos utilizados foram os seguintes:
  Configurar o endereço IP e máscara (24 bits):

```bash
    sudo ip addr add 10.0.2.3/24 dev enp0s1
```

Ativar a interface (caso ainda não estivesse ativa):

```bash
    sudo ip link set enp0s1 up
```

Configurar o gateway padrão:

```bash
    sudo ip route add default via 10.0.2.2
```

Configurar o servidor DNS (editando o resolv.conf):

```bash
    echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

Após executar esses comandos, a interface passou a operar com o endereço IP estático 10.0.2.3, gateway 10.0.2.2, máscara /24 e DNS da Google, garantindo conectividade adequada dentro do ambiente virtualizado pelo UTM.

- **Evidência de Validação:**

```bash
# Saída do comando 'ip address show enp0s1'
    2: enp0s1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
            link/ether ea:d8:ac:44:5f:36 brd ff:ff:ff:ff:ff:ff
            inet 10.0.2.3/24 brd 10.0.2.255 scope global enp0s1
            valid_lft 3002sec preferred_lft 3002sec
            inet6 fd2c:d5a5:bb17:fbfe:e8d8:acff:fe44:5f36/64 scope global mngtmpaddr
            valid_lft 2591971sec preferred_lft 604771sec
            inet6 fe80::e8d8:acff:fe44:5f36/64 scope link
            valid_lft forever preferred_lft forever
# Saída do comando 'ip route'
    default via 10.0.2.2 dev enp0s1
    10.0.2.0/24 dev enp0s1 proto kernel scope link src 10.0.2.3
# Saída do comando 'cat /etc/network/interfaces'
    # This file describes the network interfaces available on your system
    # and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # The primary network interface
    allow-hotplug enp0s1
    iface enp0s1 inet dhcp
    # This is an autoconfigured IPv6 interface
    iface enp0s1 inet6 auto
```

#### Prática 0002 checkpoint05 (Livro-Texto p. 288)

- **Resumo da Prática:** Realizei o download do arquivo install.py a partir do endereço disponibilizado em:
  http://www.aied.com.br/linux/download/install.py.
  Para isso, utilizei o comando wget, que permite baixar arquivos diretamente pela linha de comando. O comando utilizado foi:

```bash
wget -P /tmp/install.py http://www.aied.com.br/linux/download/install.py
```

Após a conclusão do download, o arquivo foi salvo com o nome install.py dentro do diretório /tmp, garantindo que ficasse armazenado em uma área temporária adequada para execução ou manipulação posterior.

- **Evidência de Validação:**

```bash
# Saída do comando 'cat /tmp/install.py'
    #!/usr/bin/python3
    import os;
    import sys
    import platform
    machine2bits = {'AMD64': 64, 'x86_64': 64, 'i386': 32, 'x86': 32, 'i686' : 32}
    os_version = machine2bits.get(platform.machine(), None)

    os.system("apt update");
    os.system("wget -O /tmp/libjsoncpp1_1.7.4-3_amd64.deb http://ftp.br.debian.org/debian/pool/main/libj/libjsoncpp/libjsoncpp1_1.7.4-3_amd64.deb");
    os.system("dpkg -i /tmp/libjsoncpp1_1.7.4-3_amd64.deb");
    #os.system("apt install libjsoncpp-dev -y");
    os.system("apt install g++ -y");
    os.system("apt install libcurl4-openssl-dev -y");
    os.system("rm -r /etc/aied");
    os.system("rm -r /etc/aied");
    os.system("mkdir /etc/aied");
    os.system("wget -O /tmp/aied.tar.gz http://www.aied.com.br/linux/download/aied_"+ str(os_version) +".tar.gz" );
    os.system("tar -xzvf /tmp/aied.tar.gz -C /etc/aied/");
    #os.system("rm /usr/sbin/aied");
    #os.system("rm /usr/bin/aied.py");
    os.system("ln -s /etc/aied/aied_"+ str(os_version) +" /usr/bin/aied");
    os.system("chmod +x /etc/aied/aied_"+ str ( os_version ) + "   " );

    #OK, será usado para isntalacao do aied.com.br
```

---

## ATIVIDADE 2: Mapa Mental (Conceitos Chave)

- **Instrução:** Esta atividade é **física** e **manual**.
- **Tarefa:** Crie um Mapa Mental em uma **única folha A4**, feito à mão, conectando os
  conceitos centrais dos Capítulos 6, 7 e 9. Não pode ser feito a lápis.
- **Entrega:** Entregar a folha A4 fisicamente ao professor antes da prova, até o dia
  **28/11/2025**.

---

## ATIVIDADE 3: Análise e Compilação dos Códigos

**Instrução:** Para cada programa listado abaixo, você deve:

1. Colar o código-fonte limpo (sem números de linha).
2. Compilar e executar o código no seu terminal.
3. Colar a saída exata que você obteve.
4. Escrever uma breve análise do que a saída significa e se corresponde ao objetivo do
   código.

### Códigos do Capítulo 6 (Discos e Montagem)

#### `devices.cpp` (Livro-Texto p. 151-152)

- **Objetivo do Código:** Ler o arquivo virtual `/proc/mounts` para descobrir e imprimir qual
  dispositivo de bloco (ex: `/dev/sda1`) está atualmente montado no diretório raiz (`/`).
- **Código-Fonte:**

```cpp
#include <iostream>
#include <fstream>
#include <optional>
#include <string>
std::optional<std::string> get_device_of_mount_point(std::string path) {
std::ifstream mounts("/proc/mounts");
std::string mountPoint;
std::string device;
while (mounts >> device >> mountPoint) {
if (mountPoint == path)
return device;
}
return std::nullopt;
}
int main() {
if (const auto device = get_device_of_mount_point("/"))
std::cout << *device << "\n";
else
std::cout << "Not found\n";
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o devices devices.cpp -std=c++17`
- _Saída da Execução:_

```bash
    /dev/sda2
```

- _Breve Descrição:_ (Explique o que a saída significa. O dispositivo que apareceu (ex: `/dev/sda1`) é
  o que você esperava para a sua partição raiz? Por quê?)

#### `getuuid.c` (Livro-Texto p. 161-162)

- **Objetivo do Código:** Usar a biblioteca `libblkid` para listar todas as partições de um disco
  (ex: `/dev/sda`) e imprimir seus atributos, como **UUID**, **LABEL** e **TYPE**.
- **Código-Fonte:**

```c
#include <stdio.h>
#include <string.h>
#include <err.h>
#include <blkid/blkid.h>
int main (int argc, char *argv[]) {
if (argc != 2) {
fprintf(stderr, "Uso: %s <dispositivo>\nEx: %s /dev/sda\n", argv[0], argv[0]);
return 1;
}
blkid_probe pr = blkid_new_probe_from_filename(argv[1]);
if (!pr) {
err(1, "Falha ao abrir %s", argv[1]);
}
blkid_partlist ls;
int nparts, i;
ls = blkid_probe_get_partitions(pr);
if (!ls) {
err(1, "Falha ao obter partições de %s", argv[1]);
}
nparts = blkid_partlist_numof_partitions(ls);
printf("Número de partições em %s: %d\n", argv[1], nparts);
const char *uuid, *label, *type;
for (i = 0; i < nparts; i++) {
char dev_name[20];
// (Cria o nome da partição, ex: /dev/sda + 1 = /dev/sda1)
sprintf(dev_name, "%s%d", argv[1], (i+1));
blkid_probe pr_part = blkid_new_probe_from_filename(dev_name);
if (!pr_part) continue;
blkid_do_probe(pr_part);
blkid_probe_lookup_value(pr_part, "UUID", &uuid, NULL);
blkid_probe_lookup_value(pr_part, "LABEL", &label, NULL);
blkid_probe_lookup_value(pr_part, "TYPE", &type, NULL);
printf(" Partição: %s, UUID=%s, LABEL=%s, TYPE=%s\n", dev_name,
(uuid ? uuid : "null"), (label ? label : "null"), (type ? type : "null"));
blkid_free_probe(pr_part);
}
blkid_free_probe(pr);
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `gcc -o getuuid getuuid.c -lblkid`
- _Saída da Execução:_ (Execute com `sudo ./getuuid /dev/sda`)

```bash
    /dev/sda2
```

- _Breve Descrição:_ O código identidfica e imprime qual dispositivo está montado na raiz do sistema, para isso ele executa uma função responsável por receber o caminho de ponto de montagem, abrir o arquivo /proc/mounts e realizar a leitura do arquivo, procurando pelo ponto de montagem desejado (/), se o mountPoint lido for igual ao path da função, ele retorna o nome do dispositivo encontrado.

```c++
    if (mountPoint == path)
        return device;
```

O retorno desse executável é /dev/sda2 (ou /dev/sda1 para sistemas MBR), que é justamente o dispositivo que montou o caminho (/).

---

### Códigos do Capítulo 7 (Processos)

#### `teste.c` (Livro-Texto p. 181-182)

- **Objetivo do Código:** Um programa "Olá, Mundo" simples para demonstrar o ciclo
  completo de compilação do GCC (Pré-processamento, Compilação, Montagem, Ligação).
- **Código-Fonte:**

```c
#include <stdio.h>
int main() {
printf("Aied é 10, Aied é TOP, tá no Youtube\n");
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `gcc -o teste teste.c`
- _Saída da Execução:_

```bash
Aied é 10, Aied é TOP, tá no Youtube
```

- _Breve Descrição:_ O executável exibe no prompt o texto "Aied é 10, Aied é TOP, tá no Youtube" utilizando o comando:

```cpp
printf("Aied é 10, Aied é TOP, tá no Youtube\n");
```

#### `myblkid.cpp` (Livro-Texto p. 186-187)

- **Objetivo do Código:** Demonstrar como um programa C++ pode usar a biblioteca `libblkid`
  (uma biblioteca C) para obter o UUID de uma partição específica (neste caso, `/dev/sda1`).
- **Código-Fonte:**

```cpp
#include <iostream>
#include <blkid/blkid.h>
#include <err.h>
#include <string>
int main (int argc, char *argv[]) {
blkid_probe pr;
const char *uuid;
std::string partition = "/dev/sda1"; // Codificado no fonte
pr = blkid_new_probe_from_filename(partition.c_str());
if (!pr) {
err(2, "Falha ao abrir %s", partition.c_str());
}
blkid_do_probe(pr);
blkid_probe_lookup_value(pr, "UUID", &uuid, NULL);
printf("UUID=%s\n", (uuid ? uuid : "null"));
blkid_free_probe(pr);
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o myblkid myblkid.cpp -lblkid`
- _Saída da Execução:_

```bash
    UUID=CA3D-6052
```

- _Breve Descrição:_ O código exibe o UUID do disco sda1, que, na maioria dos sistemas linux legados, é o dispositivo de montagem da raiz. O UUID é recuperado e salvo na variável pr a partir da linha:

```bash
    pr = blkid_new_probe_from_filename(partition.c_str());
```

#### `calcfb.cpp` (Livro-Texto p. 187)

_(Esta prática requer dois arquivos)_

- **Objetivo do Código:** Demonstrar como criar e usar uma biblioteca de cabeçalho (`.h`)
  local. O `calcfb.cpp` (programa principal) incluirá `fibonacci.h` (biblioteca) para calcular um
  número da sequência.
- **Código-Fonte (`fibonacci.h`):**

```cpp
// (p. 187)
int fibonacci(int n) {
if (n <= 1) return n;
return fibonacci(n - 1) + fibonacci(n - 2);
}
```

- **Código-Fonte (`calcfb.cpp`):**

```cpp
// (p. 187)
#include <stdio.h>
#include "fibonacci.h" // Aspas "" para incluir um arquivo local
int main(int argc, char* argv[]) {
printf("F%d: %d \n", 4, fibonacci(4));
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o calcfb calcfb.cpp`
- _Saída da Execução:_

```bash
    F4: 3
```

- _Breve Descrição:_ A saída foi F4: 3, indicando que o fibonacci de 4 é igual a 3. A sequência de fibonacci se dá pela soma do número atual com o anterior, considerando que começa com 1, a sequência será:
  0 + 1 = 1 + 1 = 2 + 1 = 3

#### `thread.cpp` (Livro-Texto p. 190)

- **Objetivo do Código:** Demonstrar a criação de múltiplas threads que executam
  concorrentemente com a thread principal (`main`).
- **Código-Fonte:**

```cpp
// (p. 190)
#include <iostream>
#include <thread>
#include <string>
using namespace std;
// (Função de exemplo baseada no livro)
void task1(string msg) {
cout << "A thread está falando: " << msg << endl;
}
int main() {
thread t1(task1, "Olá");
cout << "A 'main' executou..." << endl;
t1.join(); // A main espera a thread t1 terminar
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ thread.cpp -o thread -pthread -std=c++11`
- _Saída da Execução:_

```bash
    A 'main' executou...
    A thread está falando: Olá
```

- _Breve Descrição:_ A linha impressa primeiro foi a linha "A 'main' executou..." pois o t1.join está abaixo do cout. O t1.join() faz o fluxo do método main esperar a thread terminar de executar para assim prosseguir.

#### `usefork.cpp` (Livro-Texto p. 191)

- **Objetivo do Código:** Demonstrar a chamada `fork()`. O programa se clona; o pai e o filho
  executam o _mesmo_ código, mas alteram variáveis diferentes, provando que têm espaços de
  memória separados.
- **Código-Fonte:**

```cpp
// (p. 191)
#include <iostream>
#include <string>
#include <unistd.h> // Para fork()
#include <stdlib.h> // Para exit()
using namespace std;
int variavelGlobal = 2; // (p. 191, linha 5)
int main() {
string identidade;
int variavelFuncao = 20; // (p. 191, linha 9)
pid_t pID = fork(); // (p. 191, linha 11)
if (pID == 0) {
// Processo filho
identidade = "Processo filho: ";
variavelGlobal++;
variavelFuncao++;
}
else if (pID < 0) {
// Erro
cerr << "Failed to fork" << endl;
exit(1);
}
else {
// Processo pai
identidade = "Processo pai:";
}
// Este código é executado por AMBOS
cout << identidade;
cout << " Variavel Global: " << variavelGlobal;
cout << " Variável Funcao: " << variavelFuncao << endl;
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o usefork usefork.cpp`
- _Saída da Execução:_

```bash
    Processo pai: Variavel Global: 2 Variável Funcao: 20
    userlinux@debian:~$ Processo filho:  Variavel Global: 3 Variável Funcao: 21
```

- _Breve Descrição:_
  Após o fork(), o processo pai e o processo filho passam a executar cópias independentes do mesmo espaço de memória, ou seja, cada um recebe sua própria cópia da variavelGlobal e da variavelFuncao. Quando o filho executa variavelGlobal++ e variavelFuncao++, ele altera apenas suas próprias cópias, sem afetar as versões do pai. Por isso, os valores impressos diferem entre pai e filho: o filho exibe valores incrementados, enquanto o pai mostra os originais. Quanto à ordem de término, o processo filho termina primeiro, pois ele executa menos instruções (não passa pela lógica do pai).

#### `usewait.cpp` (Livro-Texto p. 193)

- **Objetivo do Código:** Demonstrar a chamada `wait()`. O processo-pai usa `wait(NULL)`
  para pausar sua própria execution e aguardar que o processo-filho termine antes de
  continuar.
- **Código-Fonte:**

```cpp
// (p. 193)
#include <iostream>
#include <unistd.h>
#include <sys/wait.h> // (p. 193, linha 3)
using namespace std;
int main() {
pid_t pID = fork();
pid_t cpid;
if ( pID == 0 ) {
// Filho
cout << "Saindo do processo filho. " << endl;
return 0;
} else if (pID > 0) {
// Pai
cout << "Pai esperando o filho terminar..." << endl;
cpid = wait(NULL); // (p. 193, linha 17)
} else {
// Erro
cerr << "Failed to fork" << endl;
return 1;
}
cout << "PID do pai: " << getpid() << endl;
cout << "PID do filho (retornado por wait): " << cpid << endl;
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o usewait usewait.cpp`
- _Saída da Execução:_

```bash
Pai esperando o filho terminar...
Saindo do processo filho.
PID do pai: 763
PID do filho (retornado por wait): 764
```

- _Breve Descrição:_ Sim, a linha "Pai esperando o filho terminar..." sempre aparece antes de "PID do pai..." porque o pai chama wait(NULL), ficando bloqueado até o processo filho terminar; somente depois disso ele continua e imprime seu PID. O PID do filho é impresso pelo processo pai porque wait() retorna exatamente o identificador do processo filho que acabou de terminar, permitindo ao pai saber qual processo ele aguardou.

#### `usewait_exit.cpp` (Livro-Texto p. 194)

- **Objetivo do Código:** Expandir o `wait()`, mostrando como o pai pode capturar o _código de
  saída_ (status) do filho, usando `WIFEXITED` e `WEXITSTATUS`.
- **Código-Fonte:**

```c
// (p. 194) (Este código é C, não C++)
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
#include <signal.h> // Para psignal
int main() {
pid_t pid;
int stat; // (p. 194, linha 11)
pid = fork();
if(pid == 0) {
// Filho
printf("Saindo do processo filho.\n");
exit(1); // (p. 194, linha 15)
}
else if (pid > 0) {
// Pai
wait(&stat); // (p. 194, linha 17 - modificado do NULL)
if(WIFEXITED(stat)) { // (p. 194, linha 20)
printf("WEXIT: %d\n", WEXITSTATUS(stat)); // (p. 194, linha 21)
}
else if (WIFSIGNALED(stat)) {
psignal(WTERMSIG(stat), "Sinal de saída: ");
}
printf("PID do pai: %d\n", getpid());
printf("PID do filho: %d\n", pid);
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `gcc -o usewait_exit usewait_exit.cpp`
- _Saída da Execução:_

```bash
    Saindo do processo filho.
    WEXIT: 1
    PID do pai: 773
    PID do filho: 774
```

- _Breve Descrição:_ O valor impresso por WEXITSTATUS(stat) foi 1, porque o processo filho chamou explicitamente exit(1) ao terminar; assim, quando o pai executa wait(&stat), ele recebe no parâmetro stat as informações do término do filho, incluindo o código de saída fornecido ao exit(), e WEXITSTATUS(stat) extrai exatamente esse valor — por isso o pai imprime 1 como status de saída.

#### `waitpid.cpp` (Livro-Texto p. 195)

- **Objetivo do Código:** Demonstrar o `waitpid()` para gerenciar _múltiplos_ filhos. O pai cria 5
  filhos, e então espera por _cada um_ deles especificamente, coletando seus códigos de saída
  (100 a 104).
- **Código-Fonte:**

```cpp
// (p. 195)
#include <iostream>
#include <sys/wait.h>
#include <unistd.h>
using namespace std;
int main() {
pid_t pid[5];
int i, stat;
for (i = 0; i < 5; i++) {
pid[i] = fork(); // (p. 195, linha 11)
if( pid[i] == 0) {
// Filho
sleep(1); // (p. 195, linha 14)
exit(100 + i); // (p. 195, linha 15)
}
}
for (i = 0; i < 5; i++) {
pid_t cpid = waitpid(pid[i], &stat, 0); // (p. 195, linha 19)
if (WIFEXITED(stat)) {
cout << "O filho " << cpid << " terminou com o status: "
<< WEXITSTATUS(stat) << endl;
}
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o waitpid waitpid.cpp`
- _Saída da Execução:_

```bash
    O filho 782 terminou com o status: 100
    O filho 783 terminou com o status: 101
    O filho 784 terminou com o status: 102
    O filho 785 terminou com o status: 103
    O filho 786 terminou com o status: 104
```

- _Breve Descrição:_ A saída mostra que os PIDs dos filhos (782–786) apareceram exatamente na ordem em que foram criados, e os códigos de status (100–104) também foram exibidos na sequência correspondente. Isso acontece porque o programa usa `waitpid(pid[i], ...)`, que faz o processo pai esperar cada filho específico seguindo a ordem do array `pid[]`; diferente de `wait()`, que aguardaria qualquer filho que terminasse primeiro. Assim, mesmo que a ordem real de término possa variar no sistema operacional, o `waitpid()` força o pai a coletar os resultados de cada filho na ordem desejada.

#### `system.cpp` (Livro-Texto p. 196)

- **Objetivo do Código:** Demonstrar a função `system()`, que é um atalho (e geralmente
  inseguro) para `fork + exec + wait`. O programa C++ pausa, executa um comando de shell (`ls
-l`) e depois continua.
- **Código-Fonte:**

```cpp
// (p. 196)
#include <iostream>
#include <stdlib.h> // Para system()
int main() {
system("ls -l");
std::cout << "Executado" << std::endl;
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o system system.cpp`
- _Saída da Execução:_

```bash
    total 288
    -rwxr-xr-x 1 userlinux userlinux 16000 Nov 14 22:22 calcfb
    -rw-r--r-- 1 userlinux userlinux   182 Nov 14 22:22 calcfb.cpp
    drwxr-xr-x 2 root      root       4096 Oct 24 22:54 cdrom
    drwxr-xr-x 2 userlinux userlinux  4096 Nov 14 21:50 compilacao
    -rwxr-xr-x 1 userlinux userlinux 30840 Nov 14 21:51 devices
    -rw-r--r-- 1 root      root        471 Nov 14 21:50 devices.cpp
    -rw-r--r-- 1 userlinux userlinux   103 Nov 14 22:22 fibonacci.h
    -rw-r--r-- 1 userlinux userlinux   795 Nov  7 22:35 install_curl.py
    -rw-r--r-- 1 userlinux userlinux  1062 Sep  7  2022 install.py
    -rw-r--r-- 1 userlinux userlinux  1062 Sep  7  2022 install.py.1
    -rw-r--r-- 1 root      root       1062 Sep  7  2022 install.py.2
    -rw-r--r-- 1 root      root       1062 Sep  7  2022 install.py.3
    -rwxr-xr-x 1 userlinux userlinux 24168 Nov 14 22:10 myblkid
    -rw-r--r-- 1 root      root        489 Nov 14 22:10 myblkid.cpp
    -rwxr-xr-x 1 userlinux userlinux 16600 Nov 14 22:41 system
    -rw-r--r-- 1 userlinux userlinux   150 Nov 14 22:41 system.cpp
    -rwxr-xr-x 1 userlinux userlinux 15960 Nov 14 22:07 teste
    -rw-r--r-- 1 root      root         97 Nov 14 22:06 teste.c
    -rwxr-xr-x 1 userlinux userlinux 26288 Nov 14 22:26 thread
    -rw-r--r-- 1 userlinux userlinux   349 Nov 14 22:26 thread.cpp
    -rw-r--r-- 1 userlinux userlinux   502 Nov 14 21:47 typescript
    -rwxr-xr-x 1 userlinux userlinux 17496 Nov 14 22:30 usefork
    -rw-r--r-- 1 userlinux userlinux   703 Nov 14 22:30 usefork.cpp
    -rwxr-xr-x 1 userlinux userlinux 16800 Nov 14 22:35 usewait
    -rw-r--r-- 1 userlinux userlinux   537 Nov 14 22:35 usewait.cpp
    -rwxr-xr-x 1 userlinux userlinux 16272 Nov 14 22:38 usewait_exit
    -rw-r--r-- 1 userlinux userlinux   641 Nov 14 22:37 usewait_exit.cpp
    -rwxr-xr-x 1 userlinux userlinux 16792 Nov 14 22:39 waitpid
    -rw-r--r-- 1 userlinux userlinux   515 Nov 14 22:39 waitpid.cpp
    Executado
```

- _Breve Descrição:_ A saída exibida no terminal mostra o resultado do comando ls -l, ou seja, a lista detalhada de arquivos do diretório, e somente depois aparece a palavra “Executado”. Isso ocorre porque system("ls -l") faz o programa aguardar o término do comando externo antes de continuar; o processo só prossegue para o std::cout após o ls terminar completamente.

#### `pop.cpp` (Livro-Texto p. 197)

- **Objetivo do Código:** Demonstrar a função `popen()` (pipe open). Similar ao `system()`, ele
  executa um comando, mas permite ao programa C++ _capturar_ a saída do comando (`ls -l`) e
  processá-la linha por linha.
- **Código-Fonte:**

```cpp
// (p. 197)
#include <iostream>
#include <stdio.h> // Para popen, pclose, FILE
#include <stdlib.h> // Para exit
int main() {
FILE *fpipe;
char *command = (char *)"ls -l";
char line[256];
if ( !(fpipe = (FILE*)popen(command,"r")) ) { // (p. 197, linha 9)
perror("Falha ao abrir um pipe");
exit(1);
}
while ( fgets( line, sizeof line, fpipe)) { // (p. 197, linha 13)
std::cout << "Linha: " << line; // line já contém \n
}
pclose(fpipe);
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o pop pop.cpp`
- _Saída da Execução:_

```bash
    Linha: total 312
    Linha: -rwxr-xr-x 1 userlinux userlinux 16000 Nov 14 22:22 calcfb
    Linha: -rw-r--r-- 1 userlinux userlinux   182 Nov 14 22:22 calcfb.cpp
    Linha: drwxr-xr-x 2 root      root       4096 Oct 24 22:54 cdrom
    Linha: drwxr-xr-x 2 userlinux userlinux  4096 Nov 14 21:50 compilacao
    Linha: -rwxr-xr-x 1 userlinux userlinux 30840 Nov 14 21:51 devices
    Linha: -rw-r--r-- 1 root      root        471 Nov 14 21:50 devices.cpp
    Linha: -rw-r--r-- 1 userlinux userlinux   103 Nov 14 22:22 fibonacci.h
    Linha: -rw-r--r-- 1 userlinux userlinux   795 Nov  7 22:35 install_curl.py
    Linha: -rw-r--r-- 1 userlinux userlinux  1062 Sep  7  2022 install.py
    Linha: -rw-r--r-- 1 userlinux userlinux  1062 Sep  7  2022 install.py.1
    Linha: -rw-r--r-- 1 root      root       1062 Sep  7  2022 install.py.2
    Linha: -rw-r--r-- 1 root      root       1062 Sep  7  2022 install.py.3
    Linha: -rwxr-xr-x 1 userlinux userlinux 24168 Nov 14 22:10 myblkid
    Linha: -rw-r--r-- 1 root      root        489 Nov 14 22:10 myblkid.cpp
    Linha: -rwxr-xr-x 1 userlinux userlinux 16624 Nov 14 22:44 pop
    Linha: -rw-r--r-- 1 userlinux userlinux   449 Nov 14 22:44 pop.cpp
    Linha: -rwxr-xr-x 1 userlinux userlinux 16600 Nov 14 22:41 system
    Linha: -rw-r--r-- 1 userlinux userlinux   150 Nov 14 22:41 system.cpp
    Linha: -rwxr-xr-x 1 userlinux userlinux 15960 Nov 14 22:07 teste
    Linha: -rw-r--r-- 1 root      root         97 Nov 14 22:06 teste.c
    Linha: -rwxr-xr-x 1 userlinux userlinux 26288 Nov 14 22:26 thread
    Linha: -rw-r--r-- 1 userlinux userlinux   349 Nov 14 22:26 thread.cpp
    Linha: -rw-r--r-- 1 userlinux userlinux   502 Nov 14 21:47 typescript
    Linha: -rwxr-xr-x 1 userlinux userlinux 17496 Nov 14 22:30 usefork
    Linha: -rw-r--r-- 1 userlinux userlinux   703 Nov 14 22:30 usefork.cpp
    Linha: -rwxr-xr-x 1 userlinux userlinux 16800 Nov 14 22:35 usewait
    Linha: -rw-r--r-- 1 userlinux userlinux   537 Nov 14 22:35 usewait.cpp
    Linha: -rwxr-xr-x 1 userlinux userlinux 16272 Nov 14 22:38 usewait_exit
    Linha: -rw-r--r-- 1 userlinux userlinux   641 Nov 14 22:37 usewait_exit.cpp
    Linha: -rwxr-xr-x 1 userlinux userlinux 16792 Nov 14 22:39 waitpid
    Linha: -rw-r--r-- 1 userlinux userlinux   515 Nov 14 22:39 waitpid.cpp
```

- _Breve Descrição:_ A diferença é que, neste programa, a saída do ls -l não é exibida diretamente pelo terminal como no system(). Em vez disso, o popen() captura a saída do comando e permite que o programa leia cada linha individualmente usando fgets(). Assim, o programa imprime cada linha precedida de "Linha: ". Ou seja, o popen permitiu interceptar e processar a saída do ls -l dentro do próprio código, linha por linha — algo impossível com system(), que apenas executa o comando e despeja sua saída diretamente na tela.

#### `receivesignal.cpp` (Livro-Texto p. 203)

- **Objetivo do Código:** Demonstrar como um processo pode "capturar" (handle) um sinal.
  Este programa entra em loop infinito, mas se o usuário pressionar `Ctrl+C` (que envia o sinal
  `SIGINT`), o programa executa a função `signal_handler` em vez de fechar imediatamente.
- **Código-Fonte:**

```cpp
// (p. 203)
#include <iostream>
#include <csignal>
#include <unistd.h>
using namespace std;
void signal_handler (int signum) { // (p. 203, linha 6)
cout << "Processo será interrompido pelo sinal: (" << signum << ")." << endl;
exit(signum);
}
int main () {
signal(SIGINT, signal_handler); // (p. 203, linha 12)
while(1) {
cout << "Dentro do laço de repetição infinito." << endl;
sleep(1);
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o receivesignal receivesignal.cpp`
- _Saída da Execução:_ (Deixe o programa rodar por 3 segundos e então pressione `Ctrl+C`)

```bash
    Dentro do laço de repetição infinito.
    Dentro do laço de repetição infinito.
    Dentro do laço de repetição infinito.
    Dentro do laço de repetição infinito.
    ^CProcesso será interrompido pelo sinal: (2).
```

- _Breve Descrição:_ Ao pressionar Ctrl+C, o programa não encerrou silenciosamente; ele chamou a função signal_handler, que imprimiu a mensagem informando que o processo seria interrompido pelo sinal recebido. Isso acontece porque signal(SIGINT, signal_handler) substitui o comportamento padrão do Ctrl+C (encerrar imediatamente) por uma função personalizada. O número do sinal SIGINT realmente é 2, e é esse valor que o signal_handler recebe e imprime antes de finalizar o programa.

#### `ignoresignal.cpp` (Livro-Texto p. 204)

- **Objetivo do Código:** Demonstrar como um processo pode _ignorar_ ativamente um sinal.
  Este programa é similar ao anterior, mas usa `SIG_IGN` para se tornar imune ao `Ctrl+C`
  (SIGINT).
- **Código-Fonte:**

```cpp
// (p. 204)
#include <iostream>
#include <csignal>
#include <unistd.h>
using namespace std;
int main () {
signal(SIGINT, SIG_IGN); // (p. 204, linha 8)
int i = 0;
while(1) {
cout << "Estou em loop imune... (" << ++i << ")" << endl;
sleep(1);
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o ignoresignal ignoresignal.cpp`
- _Saída da Execução:_ (Pressione `Ctrl+C` várias vezes. Para parar, use `Ctrl+\` (SIGQUIT) ou `kill -9`
  de outro terminal).

```bash
(Cole aqui a saída exata do seu terminal)
```

- _Breve Descrição:_ (O que aconteceu quando você pressionou `Ctrl+C`? O programa parou? Como
  você conseguiu parar o programa?)

#### `raisesignal.cpp` (Livro-Texto p. 204-205)

- **Objetivo do Código:** Demonstrar como um processo pode enviar um sinal _para si mesmo_
  usando a função `raise()`. O programa irá rodar por 5 segundos e então se autoenviar um
  SIGINT.
- **Código-Fonte:**

```cpp
// (p. 204-205)
#include <iostream>
#include <csignal>
#include <unistd.h>
using namespace std;
void signal_handler(int signum) {
cout << "Auto-sinal recebido: (" << signum << ")." << endl;
exit(signum);
}
int main () {
int i = 0;
signal(SIGINT, signal_handler); // (p. 205, linha 14)
while (++i) {
cout << "Dentro do laço de repetição infinito." << endl;
if( i == 5) {
raise(SIGINT); // (p. 205, linha 19)
}
sleep(1);
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o raisesignal raisesignal.cpp`
- _Saída da Execução:_

```bash
(Cole aqui a saída exata do seu terminal)
```

- _Breve Descrição:_ (O que aconteceu após 5 segundos? O programa parou sozinho? Por que a
  função `signal_handler` foi chamada?)

#### `killsignal.cpp` (Livro-Texto p. 205)

- **Objetivo do Código:** Demonstrar como um processo pode enviar um sinal para si mesmo
  usando `kill()`. É similar ao `raise()`, mas requer que o processo saiba o seu próprio PID.
- **Código-Fonte:**

```cpp
// (p. 205)
#include <iostream>
#include <csignal>
#include <unistd.h>
using namespace std;
void signal_handler(int signum) {
cout << "Processo será interrompido pelo sinal: (" << signum << ")." << endl;
exit(signum);
}
int main () {
int pid, i = 0;
pid = getpid(); // (p. 205, linha 19)
// (Nota: O livro usa SIGUSR1, vamos capturar SIGUSR1)
signal(SIGUSR1, signal_handler);
while (++i) {
cout << "Dentro do laço de repetição infinito." << endl;
if( i == 5) {
kill(pid, SIGUSR1); // (p. 205, linha 22)
}
sleep(1);
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o killsignal killsignal.cpp`
- _Saída da Execução:_

```bash
(Cole aqui a saída exata do seu terminal)
```

- _Breve Descrição:_ (O que aconteceu após 5 segundos? Qual o número do sinal `SIGUSR1` que
  apareceu na saída?)

#### `forksignal.cpp` (Livro-Texto p. 206)

- **Objetivo do Código:** Um exemplo complexo de IPC usando sinais. O processo-pai e o
  processo-filho se comunicam: o pai envia um sinal para o filho (`SIGUSR1`), e o filho envia um
  sinal de volta para o pai (`SIGUSR1`).
- **Código-Fonte:**

```cpp
// (p. 206)
#include <iostream>
#include <csignal>
#include <unistd.h>
#include <sys/wait.h>
using namespace std;
void signal_parent_handler(int signum) {
cout << "Processo (PAI) recebeu sinal: (" << signum << ")." << endl;
}
void signal_child_handler(int signum) {
cout << "Processo (FILHO) recebeu sinal: (" << signum << ")." << endl;
}
int main () {
pid_t pid;
pid = fork();
if (pid < 0) {
cerr << "Erro no fork" << endl;
return 1;
}
else if (pid == 0) {
// Processo Filho
signal(SIGUSR1, signal_child_handler);
cout << "Processo filho aguardando sinal..." << endl;
pause(); // Espera pelo sinal do pai (p. 206, linha 32)
cout << "Filho enviando sinal de volta ao pai..." << endl;
kill(getppid(), SIGUSR1); // Envia sinal para o pai (p. 206, linha 34)
exit(0);
}
else {
// Processo Pai
signal(SIGUSR1, signal_parent_handler);
sleep(2); // Garante que o filho está pronto e esperando
cout << "Pai enviando sinal para o filho " << pid << "." << endl;
kill(pid, SIGUSR1); // (p. 206, linha 38)
cout << "Processo pai aguardando resposta..." << endl;
pause(); // Espera pelo sinal do filho (p. 206, linha 40)
wait(NULL); // Limpa o filho
cout << "Pai terminando." << endl;
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o forksignal forksignal.cpp`
- _Saída da Execução:_

```bash
    Processo filho aguardando sinal...
    Pai enviando sinal para o filho 825.
    Processo (FILHO) recebeu sinal: (10).
    Filho enviando sinal de volta ao pai...
    Processo (PAI) recebeu sinal: (10).
    Processo pai aguardando resposta...
```

- _Breve Descrição:_ A sequência de eventos ocorre assim: primeiro o processo pai faz o fork(), e o filho passa a executar sua parte do código; nele, o filho instala seu handler, imprime que está aguardando e então chama pause(), ficando bloqueado até receber um sinal. Enquanto isso, o pai instala o próprio handler, espera 2 segundos para garantir que o filho já está pausado e, então, envia o sinal SIGUSR1 para o filho usando kill(pid, SIGUSR1). O filho recebe esse sinal, executa seu signal_child_handler, e após retornar do handler, continua a execução, imprime que vai enviar um sinal de volta e usa kill(getppid(), SIGUSR1) para enviar o mesmo sinal ao pai. O pai, que também estava bloqueado em pause(), recebe o sinal vindo do filho, executa seu signal_parent_handler, e só então continua, faz o wait() para limpar o processo filho e termina o programa. Ou seja: o pai enviou o sinal, o filho recebeu, respondeu, e o pause() em ambos serviu para bloquear a execução até a chegada de um sinal.

---

### Códigos do Capítulo 9 (Redes)

#### `resolveaied.cpp` (Livro-Texto p. 264)

- **Objetivo do Código:** Demonstrar uma consulta DNS básica. O programa usa a função
  `gethostbyname` para traduzir um nome de domínio (`www.aied.com.br`) em seu endereço IP.
- **Código-Fonte:**

```cpp
// (p. 264, versão simples)
#include <netdb.h>
#include <arpa/inet.h>
#include <iostream>
int main() {
struct hostent *he;
char *ip;
he = gethostbyname("[www.aied.com](https://www.aied.com).br"); // (p. 264, linha 9)
if (he) {
ip = inet_ntoa(*(struct in_addr*) he->h_addr_list[0]);
std::cout << ip << std::endl;
} else {
std::cerr << "Erro ao resolver host." << std::endl;
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o resolveaied resolveaied.cpp`
- _Saída da Execução:_

```bash
(Cole aqui a saída exata do seu terminal ao rodar ./resolveaied)
```

- _Breve Descrição:_ (Qual endereço IP foi retornado para `www.aied.com.br`?)

#### `testport.cpp` (Livro-Texto p. 276)

- **Objetivo do Código:** Testar se uma porta TCP específica (porta 80) está aberta em um
  servidor remoto (`aied.com.br`). Requer a biblioteca SFML.
- **Código-Fonte:**

```cpp
// (p. 276)
#include <iostream>
#include <SFML/Network.hpp> // (p. 276, linha 2)
#include <string>
static bool port_is_open(const std::string& address, int port) { // (p. 276, linha 5)
return (sf::TcpSocket().connect(address, port) == sf::Socket::Done);
}
int main() {
std::cout << "Testando Porta 80: aied.com.br" << std::endl; // (p. 276, linha 13)
if ( port_is_open("aied.com.br", 80) )
std::cout << "Porta está ABERTA" << std::endl;
else
std::cout << "Porta está FECHADA" << std::endl;
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o testport testport.cpp -lsfml-network -lsfml-system`
- _Saída da Execução:_

```bash
(Cole aqui a saída exata do seu terminal ao rodar ./testport)
```

- _Breve Descrição:_ (A porta 80 (HTTP) do servidor `aied.com.br` estava aberta ou fechada?)

#### `getcurl.cpp` (Livro-Texto p. 283-284)

- **Objetivo do Código:** Demonstrar como fazer o download de um arquivo (uma imagem
  `.iso`) de uma URL usando a biblioteca `libcurl` em C++.
- **Código-Fonte:**

```cpp
// (p. 283-284)
#include <stdio.h>
#include <curl/curl.h> // (p. 283, linha 2)
int main(void) {
CURL *curl;
FILE *fp;
CURLcode res;
char url[] =
"[http://www.aied.com.br/linux/download/output_image.iso](http://www.aied.com.br/linux/download/out
put_image.iso)"; // (p. 283, linha 8)
char outfilename[FILENAME_MAX] = "/tmp/output_image.iso"; // (p. 283, linha 9)
curl = curl_easy_init();
if (curl) {
fp = fopen(outfilename, "wb"); // (p. 284, linha 12)
curl_easy_setopt(curl, CURLOPT_URL, url);
curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, NULL);
curl_easy_setopt(curl, CURLOPT_WRITEDATA, fp);
res = curl_easy_perform(curl);
curl_easy_cleanup(curl);
fclose(fp);
if (res == CURLE_OK)
printf("Download concluído com sucesso para /tmp/output_image.iso\n");
else
printf("Erro no download: %s\n", curl_easy_strerror(res));
}
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o getcurl getcurl.cpp -lcurl`
- _Saída da Execução:_

```bash
(Cole aqui a saída exata do seu terminal ao rodar ./getcurl)
```

- _Breve Descrição:_ (O programa reportou sucesso? Verifique com `ls -lh /tmp/output_image.iso` se
  o arquivo realmente foi baixado e qual o seu tamanho.)

#### `postjson.cpp` (Livro-Texto p. 284-285)

- **Objetivo do Código:** Demonstrar como enviar dados (um payload JSON) para um
  servidor web usando o método `POST` com a `libcurl`.
- **Código-Fonte:**

```cpp
// (p. 284-285)
#include <stdio.h>
#include <curl/curl.h>
#include <string>
int main(void) {
CURLcode ret;
CURL *hnd;
struct curl_slist *slist1;
std::string jsonstr = "{\"username\":\"aied\",\"password\":\"123456\"}"; // (p. 284, linha 10)
slist1 = NULL;
slist1 = curl_slist_append(slist1, "Content-Type: application/json"); // (p. 284, linha 13)
hnd = curl_easy_init();
curl_easy_setopt(hnd, CURLOPT_URL,
"[http://www.aied.com.br/linux/download/echo.php](http://www.aied.com.br/linux/download/echo.php)")
; // (p. 284, linha 18)
curl_easy_setopt(hnd, CURLOPT_POSTFIELDS, jsonstr.c_str());
curl_easy_setopt(hnd, CURLOPT_USERAGENT, "curl/7.38.0");
curl_easy_setopt(hnd, CURLOPT_HTTPHEADER, slist1);
curl_easy_setopt(hnd, CURLOPT_CUSTOMREQUEST, "POST"); // (p. 285, linha 23)
ret = curl_easy_perform(hnd);
curl_easy_cleanup(hnd);
hnd = NULL;
curl_slist_free_all(slist1);
slist1 = NULL;
return 0;
}
```

- **Análise da Saída:**
- _Comando de Compilação:_ `g++ -o postjson postjson.cpp -lcurl`
- _Saída da Execução:_

```bash
(Cole aqui a saída exata do seu terminal ao rodar ./postjson)
```

- _Breve Descrição:_ (O servidor `echo.php` retornou o mesmo JSON que você enviou? O que isso
  prova sobre o método `POST`?)

#### `download.sh` (Livro-Texto p. 285-286)

- **Objetivo do Código:** Criar um script de download robusto que baixa arquivos de uma lista
  (`urls.txt`) e tenta novamente (`--retry`) em caso de falha, continuando de onde parou (`-C`).
- **Código-Fonte:**
  _(Nota: Crie o arquivo `urls.txt` em `~/Downloads/` antes, contendo a URL:
  http://www.aied.com.br/linux/download/output_image.iso)_

```bash
#!/bin/bash
# (p. 285-286)
# (Variável para o diretório de downloads do usuário atual)
DOWNLOAD_DIR="/home/$(whoami)/Downloads"
URL_FILE="$DOWNLOAD_DIR/urls.txt"
# (Cria o diretório e o arquivo se não existirem)
mkdir -p $DOWNLOAD_DIR
echo
"[http://www.aied.com.br/linux/download/output_image.iso](http://www.aied.com.br/linux/download/out
put_image.iso)" > $URL_FILE
cd $DOWNLOAD_DIR
while true
do
# (O comando xargs lê o arquivo e passa a URL para o curl)
# (Removido o parâmetro -x socks5h... para TOR, conforme nota do manual)
xargs -n 1 curl -L -O --output-dir $DOWNLOAD_DIR -k --retry 999999999999 --retry-max-time 0
-C - < $URL_FILE
echo "Loop esperando 30s..."
sleep 30
done
```

- **Análise da Saída:**
- _Comando de Execução:_ `chmod +x download.sh` e depois `./download.sh` (use `Ctrl+C` para
  parar o loop)
- _Saída da Execução:_

```bash
(Cole aqui a saída exata do seu terminal ao rodar o script)
```

- _Breve Descrição:_ (O `curl` baixou o arquivo? O que o `xargs` fez? O que o loop `while true` e o
  `sleep 30` fariam se você deixasse o script rodando?)
