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
* **Resumo da Prática:** (Descreva brevemente o que você fez: adição do disco,
particionamento com `fdisk`, formatação com `mkfs.ext4` e configuração da montagem
automática no `/etc/fstab` para o diretório `/backup`).
* **Evidência de Validação:**
```bash
# Saída do comando 'cat /etc/fstab'
(Cole aqui a saída do seu 'cat /etc/fstab')
# Saída do comando 'df -h'
(Cole aqui a saída do seu 'df -h' mostrando o /backup montado)
```
#### Prática 8b65b431 02 (Livro-Texto p. 172)
* **Resumo da Prática:** (Descreva brevemente o que você fez: criação do diretório `cdrom` e
montagem manual do dispositivo `/dev/sr0` nele).
* **Evidência de Validação:**
```bash
# Saída do comando 'df -h'
(Cole aqui a saída do seu 'df -h' mostrando o /dev/sr0 montado)
# Saída do comando 'cat /home/usuario/cdrom/arquivo.txt'
(Cole aqui a saída do cat, que deve ser "AIED VIVO")
```
### Capítulo 7: Práticas de Processos
#### Prática prc0001 01 (Livro-Texto p. 233)
* **Resumo da Prática:** (Descreva brevemente o que você fez: execução dos comandos
`locale-gen`, `script`, a listagem de processos com `ps` e a filtragem por `python`).
* **Evidência de Validação:**
```bash
# Saída do comando 'cat /home/usuario/typescript' (após filtrar por 'python')
(Cole aqui a saída do seu 'ps aux | grep python' conforme capturado pelo 'typescript')
```
### Capítulo 9: Práticas de Redes
#### Prática 0002 checkpoint03 (Livro-Texto p. 286)
* **Resumo da Prática:** (Descreva brevemente o que você fez: configuração de IP estático
editando o arquivo `/etc/network/interfaces` e reiniciando a máquina).
* **Evidência de Validação:**
```bash
# Saída do comando 'ip address show enp0s3'
(Cole aqui a saída do 'ip address' mostrando o IP 10.0.2.3)
# Saída do comando 'ip route'
(Cole aqui a saída do 'ip route' mostrando o gateway 10.0.2.2)
# Saída do comando 'cat /etc/network/interfaces'
(Cole aqui o conteúdo do seu arquivo /etc/network/interfaces)
```
#### Prática 0002 checkpoint04 (Livro-Texto p. 287)
* **Resumo da Prática:** (Descreva brevemente o que você fez: configuração da rede para
DHCP no arquivo e, em seguida, configuração de IP estático via comandos `ip address` e `ip
route`).
* **Evidência de Validação:**
```bash
# Saída do comando 'ip address show enp0s3'
(Cole aqui a saída do 'ip address' mostrando o IP 10.0.2.3)
# Saída do comando 'ip route'
(Cole aqui a saída do 'ip route' mostrando o gateway 10.0.2.2)
# Saída do comando 'cat /etc/network/interfaces'
(Cole aqui o conteúdo do seu /etc/network/interfaces mostrando a configuração DHCP)
```
#### Prática 0002 checkpoint05 (Livro-Texto p. 288)
* **Resumo da Prática:** (Descreva brevemente o que você fez: download de um arquivo
usando `wget` para o diretório `/tmp`).
* **Evidência de Validação:**
```bash
# Saída do comando 'cat /tmp/install.py'
(Cole aqui a saída do cat, que deve ser "aied.com.br")
```
---
## ATIVIDADE 2: Mapa Mental (Conceitos Chave)
* **Instrução:** Esta atividade é **física** e **manual**.
* **Tarefa:** Crie um Mapa Mental em uma **única folha A4**, feito à mão, conectando os
conceitos centrais dos Capítulos 6, 7 e 9. Não pode ser feito a lápis.
* **Entrega:** Entregar a folha A4 fisicamente ao professor antes da prova, até o dia
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
* **Objetivo do Código:** Ler o arquivo virtual `/proc/mounts` para descobrir e imprimir qual
dispositivo de bloco (ex: `/dev/sda1`) está atualmente montado no diretório raiz (`/`).
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o devices devices.cpp -std=c++17`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar ./devices)
```
* *Breve Descrição:* (Explique o que a saída significa. O dispositivo que apareceu (ex: `/dev/sda1`) é
o que você esperava para a sua partição raiz? Por quê?)
#### `getuuid.c` (Livro-Texto p. 161-162)
* **Objetivo do Código:** Usar a biblioteca `libblkid` para listar todas as partições de um disco
(ex: `/dev/sda`) e imprimir seus atributos, como **UUID**, **LABEL** e **TYPE**.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `gcc -o getuuid getuuid.c -lblkid`
* *Saída da Execução:* (Execute com `sudo ./getuuid /dev/sda`)
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (A saída listou corretamente suas partições, como `sda1` e `sda5`? Os tipos
`ext4` e `swap` correspondem ao que você viu no `lsblk -f`?)
---
### Códigos do Capítulo 7 (Processos)
#### `teste.c` (Livro-Texto p. 181-182)
* **Objetivo do Código:** Um programa "Olá, Mundo" simples para demonstrar o ciclo
completo de compilação do GCC (Pré-processamento, Compilação, Montagem, Ligação).
* **Código-Fonte:**
```c
#include <stdio.h>
int main() {
printf("Aied é 10, Aied é TOP, tá no Youtube\n");
return 0;
}
```
* **Análise da Saída:**
* *Comando de Compilação:* `gcc -o teste teste.c`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar ./teste)
```
* *Breve Descrição:* (O programa imprimiu a string esperada no terminal?)
#### `myblkid.cpp` (Livro-Texto p. 186-187)
* **Objetivo do Código:** Demonstrar como um programa C++ pode usar a biblioteca `libblkid`
(uma biblioteca C) para obter o UUID de uma partição específica (neste caso, `/dev/sda1`).
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o myblkid myblkid.cpp -lblkid`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar sudo ./myblkid)
```
* *Breve Descrição:* (A saída corresponde ao UUID da sua partição `sda1` que você viu no `lsblk
-f`?)
#### `calcfb.cpp` (Livro-Texto p. 187)
*(Esta prática requer dois arquivos)*
* **Objetivo do Código:** Demonstrar como criar e usar uma biblioteca de cabeçalho (`.h`)
local. O `calcfb.cpp` (programa principal) incluirá `fibonacci.h` (biblioteca) para calcular um
número da sequência.
* **Código-Fonte (`fibonacci.h`):**
```cpp
// (p. 187)
int fibonacci(int n) {
if (n <= 1) return n;
return fibonacci(n - 1) + fibonacci(n - 2);
}
```
* **Código-Fonte (`calcfb.cpp`):**
```cpp
// (p. 187)
#include <stdio.h>
#include "fibonacci.h" // Aspas "" para incluir um arquivo local
int main(int argc, char* argv[]) {
printf("F%d: %d \n", 4, fibonacci(4));
return 0;
}
```
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o calcfb calcfb.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar ./calcfb)
```
* *Breve Descrição:* (A saída foi `F4: 3`? Explique por que o resultado é 3 e não 4, com base na
sequência de Fibonacci).
#### `thread.cpp` (Livro-Texto p. 190)
* **Objetivo do Código:** Demonstrar a criação de múltiplas threads que executam
concorrentemente com a thread principal (`main`).
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ thread.cpp -o thread -pthread -std=c++11`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar ./thread)
```
* *Breve Descrição:* (Qual linha imprimiu primeiro, "A 'main' executou..." ou "A thread está
falando..."? O que `t1.join()` faz?)
#### `usefork.cpp` (Livro-Texto p. 191)
* **Objetivo do Código:** Demonstrar a chamada `fork()`. O programa se clona; o pai e o filho
executam o *mesmo* código, mas alteram variáveis diferentes, provando que têm espaços de
memória separados.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o usefork usefork.cpp`
* *Saída da Execução:*
```bash
(Cole aqui as DUAS linhas de saída do seu terminal)
```
* *Breve Descrição:* (Explique por que a `variavelGlobal` e a `variavelFuncao` têm valores diferentes
para o pai e para o filho. Qual processo (pai ou filho) terminou primeiro na sua execução?)
#### `usewait.cpp` (Livro-Texto p. 193)
* **Objetivo do Código:** Demonstrar a chamada `wait()`. O processo-pai usa `wait(NULL)`
para pausar sua própria execution e aguardar que o processo-filho termine antes de
continuar.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o usewait usewait.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (A linha "Pai esperando..." sempre aparece antes de "PID do pai..."? Por que o
PID do filho é impresso pelo processo-pai?)
#### `usewait_exit.cpp` (Livro-Texto p. 194)
* **Objetivo do Código:** Expandir o `wait()`, mostrando como o pai pode capturar o *código de
saída* (status) do filho, usando `WIFEXITED` e `WEXITSTATUS`.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `gcc -o usewait_exit usewait_exit.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (Qual foi o status de saída impresso pelo `WEXIT`? Por que ele imprimiu esse
valor específico?)
#### `waitpid.cpp` (Livro-Texto p. 195)
* **Objetivo do Código:** Demonstrar o `waitpid()` para gerenciar *múltiplos* filhos. O pai cria 5
filhos, e então espera por *cada um* deles especificamente, coletando seus códigos de saída
(100 a 104).
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o waitpid waitpid.cpp`
* *Saída da Execução:*
```bash
(Cole aqui as 5 linhas de saída do seu terminal)
```
* *Breve Descrição:* (Os PIDs dos filhos apareceram em ordem? Os códigos de status (100-104)
apareceram em ordem? O que `waitpid()` faz de diferente do `wait()`?)
#### `system.cpp` (Livro-Texto p. 196)
* **Objetivo do Código:** Demonstrar a função `system()`, que é um atalho (e geralmente
inseguro) para `fork + exec + wait`. O programa C++ pausa, executa um comando de shell (`ls
-l`) e depois continua.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o system system.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (O que apareceu na tela? A lista de arquivos (`ls -l`) apareceu *antes* ou
*depois* da palavra "Executado"? Por quê?)
#### `pop.cpp` (Livro-Texto p. 197)
* **Objetivo do Código:** Demonstrar a função `popen()` (pipe open). Similar ao `system()`, ele
executa um comando, mas permite ao programa C++ *capturar* a saída do comando (`ls -l`) e
processá-la linha por linha.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o pop pop.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (Qual a diferença da saída deste programa para a saída do `system.cpp`? O
que o `popen` permitiu fazer com a saída do `ls -l`?)
#### `receivesignal.cpp` (Livro-Texto p. 203)
* **Objetivo do Código:** Demonstrar como um processo pode "capturar" (handle) um sinal.
Este programa entra em loop infinito, mas se o usuário pressionar `Ctrl+C` (que envia o sinal
`SIGINT`), o programa executa a função `signal_handler` em vez de fechar imediatamente.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o receivesignal receivesignal.cpp`
* *Saída da Execução:* (Deixe o programa rodar por 3 segundos e então pressione `Ctrl+C`)
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (O que aconteceu quando você pressionou `Ctrl+C`? O programa fechou
silenciosamente ou imprimiu a mensagem da `signal_handler`? Qual é o número do sinal `SIGINT`?)
#### `ignoresignal.cpp` (Livro-Texto p. 204)
* **Objetivo do Código:** Demonstrar como um processo pode *ignorar* ativamente um sinal.
Este programa é similar ao anterior, mas usa `SIG_IGN` para se tornar imune ao `Ctrl+C`
(SIGINT).
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o ignoresignal ignoresignal.cpp`
* *Saída da Execução:* (Pressione `Ctrl+C` várias vezes. Para parar, use `Ctrl+\` (SIGQUIT) ou `kill -9`
de outro terminal).
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (O que aconteceu quando você pressionou `Ctrl+C`? O programa parou? Como
você conseguiu parar o programa?)
#### `raisesignal.cpp` (Livro-Texto p. 204-205)
* **Objetivo do Código:** Demonstrar como um processo pode enviar um sinal *para si mesmo*
usando a função `raise()`. O programa irá rodar por 5 segundos e então se autoenviar um
SIGINT.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o raisesignal raisesignal.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (O que aconteceu após 5 segundos? O programa parou sozinho? Por que a
função `signal_handler` foi chamada?)
#### `killsignal.cpp` (Livro-Texto p. 205)
* **Objetivo do Código:** Demonstrar como um processo pode enviar um sinal para si mesmo
usando `kill()`. É similar ao `raise()`, mas requer que o processo saiba o seu próprio PID.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o killsignal killsignal.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (O que aconteceu após 5 segundos? Qual o número do sinal `SIGUSR1` que
apareceu na saída?)
#### `forksignal.cpp` (Livro-Texto p. 206)
* **Objetivo do Código:** Um exemplo complexo de IPC usando sinais. O processo-pai e o
processo-filho se comunicam: o pai envia um sinal para o filho (`SIGUSR1`), e o filho envia um
sinal de volta para o pai (`SIGUSR1`).
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o forksignal forksignal.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal)
```
* *Breve Descrição:* (Descreva a ordem dos eventos. O pai enviou o sinal? O filho recebeu? O filho
enviou de volta? O que o comando `pause()` fez em ambos os processos?)
---
### Códigos do Capítulo 9 (Redes)
#### `resolveaied.cpp` (Livro-Texto p. 264)
* **Objetivo do Código:** Demonstrar uma consulta DNS básica. O programa usa a função
`gethostbyname` para traduzir um nome de domínio (`www.aied.com.br`) em seu endereço IP.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o resolveaied resolveaied.cpp`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar ./resolveaied)
```
* *Breve Descrição:* (Qual endereço IP foi retornado para `www.aied.com.br`?)
#### `testport.cpp` (Livro-Texto p. 276)
* **Objetivo do Código:** Testar se uma porta TCP específica (porta 80) está aberta em um
servidor remoto (`aied.com.br`). Requer a biblioteca SFML.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o testport testport.cpp -lsfml-network -lsfml-system`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar ./testport)
```
* *Breve Descrição:* (A porta 80 (HTTP) do servidor `aied.com.br` estava aberta ou fechada?)
#### `getcurl.cpp` (Livro-Texto p. 283-284)
* **Objetivo do Código:** Demonstrar como fazer o download de um arquivo (uma imagem
`.iso`) de uma URL usando a biblioteca `libcurl` em C++.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o getcurl getcurl.cpp -lcurl`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar ./getcurl)
```
* *Breve Descrição:* (O programa reportou sucesso? Verifique com `ls -lh /tmp/output_image.iso` se
o arquivo realmente foi baixado e qual o seu tamanho.)
#### `postjson.cpp` (Livro-Texto p. 284-285)
* **Objetivo do Código:** Demonstrar como enviar dados (um payload JSON) para um
servidor web usando o método `POST` com a `libcurl`.
* **Código-Fonte:**
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
* **Análise da Saída:**
* *Comando de Compilação:* `g++ -o postjson postjson.cpp -lcurl`
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar ./postjson)
```
* *Breve Descrição:* (O servidor `echo.php` retornou o mesmo JSON que você enviou? O que isso
prova sobre o método `POST`?)
#### `download.sh` (Livro-Texto p. 285-286)
* **Objetivo do Código:** Criar um script de download robusto que baixa arquivos de uma lista
(`urls.txt`) e tenta novamente (`--retry`) em caso de falha, continuando de onde parou (`-C`).
* **Código-Fonte:**
*(Nota: Crie o arquivo `urls.txt` em `~/Downloads/` antes, contendo a URL:
http://www.aied.com.br/linux/download/output_image.iso)*
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
* **Análise da Saída:**
* *Comando de Execução:* `chmod +x download.sh` e depois `./download.sh` (use `Ctrl+C` para
parar o loop)
* *Saída da Execução:*
```bash
(Cole aqui a saída exata do seu terminal ao rodar o script)
```
* *Breve Descrição:* (O `curl` baixou o arquivo? O que o `xargs` fez? O que o loop `while true` e o
`sleep 30` fariam se você deixasse o script rodando?)
