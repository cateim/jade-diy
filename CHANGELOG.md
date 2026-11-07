# Changelog

Todas as mudanças notáveis do Jade DIY são documentadas neste arquivo.

O formato é baseado em [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/).
As versões seguem o esquema **CalVer `YYYY.M.R`** (ano, mês sem zero à esquerda, revisão do mês), **não** o Versionamento Semântico.

> Este changelog foi iniciado em 2026-11-06. A primeira versão resume o projeto como ele estava nessa data, reconstruída a partir do histórico do git: o repositório não tem tags, então ela leva a data do último commit.

## [Unreleased]

## [2026.7.1] - 2026-07-06

### ✨ Added

- **Guia de uso da Jade DIY** (`README.md`): a primeira inicialização, para criar ou restaurar a carteira, sempre pelo app oficial da Blockstream; depois, o uso no Windows (apps Blockstream e SideSwap pela USB), no Android (Bluetooth, com o passo de esquecer e parear de novo quando a conexão falha) e no iOS. No iOS o app da Blockstream não pareia por Bluetooth com a placa, então o guia mostra como exportar a Xpub pela Jade, com o SideSwap conectado, e importá-la por QR Code no app da Blockstream: serve só para ver o saldo em BTC, e assinar continua exigindo um app que conecte direto, como o SideSwap.
- **Firmware compilado, pronto para gravar** (`firmware/<placa>/<versão>/`, cada pasta com `bootloader.bin`, `jade.bin`, `ota_data_initial.bin` e `partition-table.bin`): T-Display e T-Display S3 nas versões `1.0.38` e `1.0.38-98`, Waveshare S3 na `1.0.38-98`. O código é o do [Jade da Blockstream](https://github.com/Blockstream/Jade), compilado com modificações locais (a versão gravada em cada `jade.bin` termina em `-dirty`): a `1.0.38` sai da tag `1.0.38`, e a `1.0.38-98` sai do commit `f8bf5d4`, o último antes da tag `1.0.39-beta2`.
- **Instalador para Windows** (`jade_instalador_v8.85.bat`): menu interativo que instala as dependências (Python, Git for Windows, o driver USB-serial CH341SER e o ESP-IDF, este por padrão na versão que o Jade indica em `main/idf_component.yml`), clona o [Jade da Blockstream](https://github.com/Blockstream/Jade) com os submódulos, aplica a configuração de placa escolhida entre as de `configs/` do Jade e compila e grava com o `idf.py`. Chegou ao repositório na v8.61, com o crédito de autoria no banner do próprio script, e foi ampliado até a v8.85.
- **Opções de compilação e gravação do instalador**: compilar do zero ou de forma incremental, compilar e gravar, ou só gravar o que já foi compilado (os binários também são copiados para a pasta `bin_jade`), além de trocar a configuração e recompilar. Antes de gravar, o instalador confere se o chip conectado é o mesmo para o qual o build foi feito. Com a configuração da TTGO T-Display, troca o flash de 4 MB para 16 MB; com a da Waveshare S3 Touch LCD2, liga o `CONFIG_JADE_USE_USB_JTAG_SERIAL`.
- **Guia para compilar uma versão específica do Jade** (`build_penultima_versao.md`, revisão 3): no Windows 11, com PowerShell 7 e ESP-IDF 5.5.4, posiciona o Jade na tag desejada (ou no commit anterior à última tag), guarda e reaplica as modificações locais com `git stash`, sincroniza os submódulos, compila e grava. Traz a variável `CMAKE_NINJA_FORCE_RESPONSE_FILE`, sem a qual o build das placas S3 quebra no limite de linha de comando do Windows, e uma tabela de erros comuns.
- **Guia para compilar e assinar uma atualização com Secure Boot** (`assinar_bin.md`): as opções do `menuconfig` de cada placa (Secure Boot V1 na T-Display, V2 na T-Display S3, modo e velocidade do flash), a assinatura do `jade.bin` com `espsecure.py sign_data` usando a mesma chave que travou a placa e a conferência dos eFuses com `espefuse.py summary`.
- **Script de publicação dos builds** (`deploy_firmware_v2.8.bat`): para quem mantém o repositório, copia os quatro `.bin` da pasta `build` do Jade de cada placa (T-Display, T-Display S3 e Waveshare S3) para `firmware/<placa>/<versão>/`, confere antes se todos existem e pergunta antes de sobrescrever uma versão já publicada.
- **Artes da marca Jade, da Blockstream** (`assets/jade/` e `assets/wallpaper.png`): logotipo, emblema e grafismos em versões para web, impressão e monocromática, além das imagens que ilustram os guias (`assets/update/` e `assets/mod/`).

### 🔧 Changed

- **Atualização pelo navegador fora de uso**: o README marca como inoperantes, desde 2025-10-22, os guias `atualizar_jade_tdisplay.md` e `atualizar_jade_tdisplays3.md`, que atualizam pelo [ESP Tool web da Espressif](https://espressif.github.io/esptool-js) o firmware das placas vendidas com Secure Boot ativo. O `jade-signed.bin` que eles pedem também não está mais em `firmware/` desde 2025-11-28, quando os builds `1.0.36` deram lugar à `1.0.38`.

### 🐛 Fixed

- **Botão travado no Windows na T-Display com CH9102F, o "bug do DTR"** (`fix_usb_bug_tdisplay.md`): o driver e apps como Blockstream e SideSwap mantêm o RTS em nível constante, o transistor Q2 conduz o tempo todo e o GPIO0, o do botão Boot, fica preso em nível baixo. O guia de 2025-10-19 resolve no hardware, trocando o resistor R19 (10 kΩ) por um capacitor de 100 nF, ou só removendo o R19, com fotos, material, passo a passo e verificação; depois da modificação, gravar exige segurar o botão do GPIO0 até a gravação começar. Desde então o README orienta só instalar e usar, e o contorno antigo (`Disable ModemHandShake` na porta do CH9102 e o "Modo de Um Botão" no SideSwap) ficou riscado.

[Unreleased]: https://github.com/cateim/jade-diy/compare/31a76e8...HEAD
[2026.7.1]: https://github.com/cateim/jade-diy/tree/31a76e8
