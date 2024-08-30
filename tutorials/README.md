<div align="justify">

# Uso do Arduino CLI para a construção de projetos na linha de comando Windows

Os exemplos vistos neste diretório serão configurados para o microcontrolador arduino UNO R3. De acordo com a Documentação oficial do arduino

<div style="padding: 0px 20px;">
"Arduino UNO é uma placa microcontroladora baseada no ATmega328P. Possui 14 pinos de entrada/saída digital (dos quais 6 podem ser usados ​​como saídas PWM), 6 entradas analógicas, um ressonador cerâmico de 16 MHz, uma conexão USB, um conector de alimentação, um conector ICSP e um botão de reset.", por Arduino e traduzido por Google.
</div>

Este estudo está sendo produzido para ser a base sólida, tanto teórica quanto prática, para um projeto mais complexo envolvendo programação com microcontroladores.

## Arduino CLI

A instalação simples, no Windows, pode ser feita por meio do **Console do Git Bash**. O passo-a-passo da intalação pode ser vista na [documentação oficial - API Arduino](https://arduino.github.io/arduino-cli/0.34/installation/), mas será mencionado os passos aqui, filtrando a principal informação da página web em questão.

### Instalação

Como informado anteriormente, a instação será por meio do console do Git. Abrindo-o no diretório desejado, localizado no computador, deve ser digitado o seguinte comando para a intalação do Arduino CLI:

```bash
curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | sh
```

Para finalizar, basta adicionar o diretório `<URI>/bin/` nas variáveis do Usuário/Sistema. Ao fazê-lo, feche abra um terminal do Windows e prime o comando `arduin-cli version` para confirmar se a ferramenta foi instalada e configurada com sucesso. Caso não tenha conseguido, é recomendado que visite a página oficial da Arduino API.

### Comandos

A sintaxe dos comandos segue as seguintes regras:

1. `arduino-cli <command>`
2. `arduino-cli <command> [flags...]`

Os **comandos** e **flags** podem ser visto ao utilizar o comando `arduino-cli help` no terminal. Dentre os comandos, vale destacar os principais:

* `arduino-cli help`: mostrar os principais comandos e flags disponíveis.
* `arduino-cli version`: mostrar a versão instalada da Arduino CLI.
* `arduino-cli board -h`: listar os comandos e tags disponíveis para *arduino-cli board*.
* `arduino-cli board list`: listar as informações de todas as placas conectadas ao computador.
* `arduino-cli board search`: lista de microcontroladores suportados, informando o nome, o FQBN e ID da plataforma.
* `arduino-cli core -h`: listar os comandos e tags disponíveis para *arduino-cli core*.
* `arduino-cli core list`: listar todos os núcleos instalados e presentes.
* `arduino-cli core install <core>`: para instalar os recursos necessários na comunicação com a placa.
* `arduino-cli sketch new <project name>`: criar um projeto arduino;
* `arduino-cli compile -h`: listar os comandos e tags disponíveis para *arduino-cli compile*.
* `arduino-cli compile -b <FQBN>`: compilar o projeto para dado microcontrolador de acordo com seu FQBN.
* `arduino-cli upload -h`: listar os comandos e tags disponíveis para *arduino-cli upload*.
* `arduino-cli upload -b <FQBN> -p <Port>`: gravar o projeto compilado para dado microcontrolador de acordo com seu FQBN por meio da porta em que ele esta conectado.

## Hello World (Blink) com Arduino UNO R3

Primeiro se deve instalar os recursos necessários para se comunicar com a placa em questão. O Arduino UNO R3 utiliza o núcleo AVR. Para a instalação deste, deve-se usar o comando `arduino-cli core install <core>`. Ao concluir a instalação, conecte o microcontrolado ao computador e utilize o comando `arduino-cli board list` para ver se a placa foi detectada. Caso não seja detectada, ela irá mostrar *"unknow"*. Neste caso, é necessário que seja instalado o núcleo da plataforma (Caso dê certo, será informado as informaçõs do microcontrolador nos campos Board Name, FQBN e Core).

```bash
Port Protocol Type              Board Name FQBN Core
COM3 serial   Serial Port (USB) Unknown
```

Para que o microcoontrolador seja identificado, é necessário instalar o núcleo da plataforma. Para isto, primeiro execute o comando `arduino-cli board listall mkr` e verifique FQBN. No caso do Arduino UNO R3, necessário instalar o núcleo `samd`, logo deverá ser executado o comando `arduino-cli core install arduino:samd`. A execução do comando para visualizar as informações da placa agora deverá informar os dados do microcontrolador Arduino UNO R3 (verifique se o núcleo foi instalado com sucesso). Caso ainda não esteja listando, utilize o comando `arduino-cli board search` e guarde o **FQBN** (identificador único que especifica a placa e o núcleo que você está usando) do microcontrolador (A do arduino UNO é *arduino:avr:uno*).

tendo instalado, crie um projeto com o comando `arduino-cli sketch new Blink`, entre no pasta com `cd Blink` e percebe-se que foi construído o "esqueleto" de uma aplicação Arduino (`type Blink.ino` no winows 11 ou utilize o `cat` caso esteja usando o git bash).

Abra o arquivo e insira o código a seguir:

```bash
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_BUILTIN, 1);
  delay(500);
  digitalWrite(LED_BUILTIN, 0);
  delay(500);
}
```

A compilação do projeto requer o FQBN do microcontrolador. Tendo-o basta usar o comando `arduino-cli compile -b <FQBN>` (troque a tag FQBN pelo FQBN do microcontrolador). Um resultado semelhante ao da imagem deve ser retornado.

<div align="center">
<img src="../src/imgs/tutorials_helloWorld_compile.png" alt="resultado da compilação do programa Blink" />
</div>

A gravação do projeto compulado depende, além do FQBN, da porta em que o microcontrolador está conectado. Para isto utilize o comando `arduino-cli board list` e guarde a informação referente a "Porta". Em seguida, execute o comando `arduino-cli upload -b <FQBN> -p <Port>`. O resultado disto deve ser algo como *"New upload port: COM3 (serial)"*.

<div align="center">
  <video controls width="250">
    <source src="../src/videos/tutorials_Blink_upload_result.mp4" type="video/mp4" />
  </video>
</div>

</div>