# Instalando v

**Importante:**

Para sistemas operacionais de 32 bits a instalação deve ser sempre feita a partir do código que é disponibilizado no Github do projeto.

Nesse método de instalação iremos clonar o repositório e realizar compilação.

**Dependências**

Para realizar a compilação da linguagem a partir do código disponibilizado no Github é necessário ter algum compilador C instalado.

**macOS**

```bash
xcode-select --install
```

**Debian/Ubuntu**

```bash
sudo apt install build-essential
```

**CentOS/RHEL**

```bash
sudo yum groupinstall "Development Tools"
```

**Fedora**

```bash
sudo dnf install @development-tools
```

**Instalação**

Antes de tudo é preciso definir o local onde o repositório será clonado.

Escolha um local onde o mesmo não será apagado, um bom local pode ser a pasta home do seu usuário.

Assim que o local estiver definido, abra um terminal e digite:

```bash
git clone https://github.com/vlang/v
```

**Nota:** Após clonar o repositório é possível renomear a pasta **v** para **.v**, assim a mesma ficará oculta.

Acesse a pasta v que foi criada e execute:

```bash
make
```

Com o fim do processo de compilação da linguagem adicione o binário da linguagem no path do sistema operacional:

```bash
sudo ./v symlink
```

Para testar a instalação e verificar se o comando v foi adicionando ao path do sistema operacional, feche o terminal e abra novamente em seguida digite:

```bash
v
```

Se o modo interativo da linguagem for aberto significa que a instalação está correta.

**Site oficial**

A instalação via site oficial consiste em fazer o download do pacote já compilado e adicionar o mesmo ao path do Microsoft Windows.

Acesse o site oficial: [https://vlang.io/](https://vlang.io/).

Selecione o sistema operacional (se o mesmo não for selecionado automaticamente).

Assim que o download terminar descompacte o arquivo.

Copie a pasta v que está dentro da pasta v_linux para um local onde a mesma não será apagada.

Acesse a pasta v, abra um terminal e adicione o binário da linguagem ao path do sistema operacional:

```bash
sudo ./v symlink
```

Para testar a instalação e verificar se o comando v foi adicionando ao path, feche o terminal, abra novamente e digite:

```bash
v
```