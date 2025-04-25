# 🧱 LFS - Progresso do Dia (25 de Abril de 2025)

## 📘 Resumo Geral
Este documento resume todas as etapas e descobertas feitas durante o estudo e montagem do sistema Linux From Scratch (LFS) no dia 25 de abril de 2025.

---

## 🔧 1. Tentativa de Instalação do Binutils (Segunda Passagem)
- Comando executado:
  ```bash
  make install
  ```
- Local: `/mnt/lfs/sources/binutils-2.44/build`
- ❌ **Erro encontrado:**
  ```
  mkdir: cannot create directory '/mnt/lfs/tools': Permission denied
  ```
- **Motivo:** Falta de permissões. Não estava logado como o usuário `lfs`.

---

## 🧱 2. Verificações no Diretório `/mnt/lfs/tools`
- Verificação e tentativa de link simbólico `lib64` → `lib`.
- Já existia o diretório, o que é aceitável.

---

## 📦 3. Manipulação das Dependências do GCC
- Tentativa de extrair e mover:
  ```bash
  tar -xf ../mpfr-4.2.1.tar.xz
  mv -v mpfr-4.2.1 mpfr
  ```
- ❌ **Erro:** Arquivo não encontrado, pois o diretório atual era incorreto.
- Corrigido após verificação com `ls`.

---

## 📁 4. Extração do GCC
- Inicialmente não havia extraído, causando erro:
  ```
  sed: can't read gcc/config/i386/t-linux64: No such file or directory
  ```
- Corrigido com:
  ```bash
  tar -xf gcc-14.2.0.tar.xz
  ```

---

## 🔍 5. Verificação do `t-linux64`
- Usou `find` para localizar o arquivo:
  ```bash
  find gcc-14.2.0 -name "t-linux64"
  ```
- Confirmado que o arquivo `gcc/config/i386/t-linux64` existe.

---

## ⚙️ 6. Configuração do GCC (Primeira Passagem)
- Criado o diretório `build`:
  ```bash
  mkdir build && cd build
  ```
- Rodado o script `../configure` com diversos parâmetros.
- ❌ Inicialmente retornou `No such file or directory` porque o local estava errado.
- Corrigido e executado no diretório correto.

---

## 🛠️ 7. Erro no `make`
- Ao rodar `make`, retornou:
  ```
  make: *** No targets specified and no makefile found.  Stop.
  ```
- Verificado que o `configure` **não gerou o Makefile**.

---

## ❌ 8. Falha na Configuração
- O `config.log` revelou o erro:
  ```
  conftest.c:11:10: fatal error: gmp.h: No such file or directory
  configure:8430: error: Building GCC requires GMP 4.2+, MPFR 3.1.0+ and MPC 0.8.0+.
  ```
- **Motivo:** As bibliotecas não estavam dentro do diretório do GCC.

---

## ✔️ 9. Entendimento da Estrutura Correta
- As dependências `mpfr`, `gmp` e `mpc` devem ser colocadas **dentro do diretório `gcc-14.2.0`**, com os nomes esperados:
  ```
  gcc-14.2.0/mpfr
  gcc-14.2.0/gmp
  gcc-14.2.0/mpc
  ```

---

## 📘 10. Comando para Gerar `limits.h`
- Comando utilizado:
  ```bash
  cat gcc/limitx.h gcc/glimits.h gcc/limity.h > \
    `dirname $($LFS_TGT-gcc -print-libgcc-file-name)`/include/limits.h
  ```
- **Objetivo:** Unificar definições de limites de tipo para o compilador temporário.

---

## ✅ Resultado do Dia
- Entendimento aprofundado dos requisitos para compilar o GCC.
- Correção de diretórios errados e estrutura dos arquivos.
- Entendimento da importância das dependências estarem corretamente integradas.
- Organização de passos para retomar o processo no próximo dia.

---

📅 **Próximo passo:** Integrar as dependências dentro do diretório `gcc-14.2.0`, reconfigurar e continuar a construção do compilador temporário.

---

📝 **Autor:** AshiraTart
📅 **Data:** 25 de Abril de 2025

