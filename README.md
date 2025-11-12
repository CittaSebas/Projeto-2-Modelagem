# Projeto-2-Modelagem
UML de um sistema para  o controle de elevadores em uma rede de hotéis

## Diagrama de Atores 
<img width="602" height="586" alt="image" src="https://github.com/user-attachments/assets/d61b406d-1e5e-49be-9a60-5a629f276e9c" />


## Casos de Uso

| Identificação | UC - 01 |
|---|---|
| Função | Registrar usuário | 
| Atores | Usuário, Controlador, Recepcionista | 
| Prioridade | Essencial | 
| Pré-Condição | Controlador autoriza o recepcionista a adicionar pessoas no sistema<br> Sistema funcionando | 
| Pós-Condição | Usuário cadastrado | 
| Fluxo Principal | * Usuário cede suas informações ao recepcionista  <br> * Recepcionista registra usuário no sistema [FS01] |
| Fluxo Secundário [FS01] | *Recepcionista registra usuário no sistema como VIP|

<br>

| Identificação | UC - 02 |
|---|---|
| Função | Ativar estado de incêndio | 
| Atores | Elevador | 
| Prioridade | Essencial | 
| Pré-Condição | Elevador funcional | 
| Pós-Condição | Elevador com portas abertas no térreo até situação normalizar | 
| Fluxo Principal | * O sensor de fumaça do predio capta fumaça <br> * Predio entra em estado de incêndio <br> * Elevador se dirige ao térreo <br> * Elevador abre as portas e permanece no terreo até a situação normalizar | 

<br>

| Identificação | UC - 03 |
|---|---|
| Função | Ativar estado de emergência | 
| Atores | Elevador, Usuário | 
| Prioridade | Essencial | 
| Pré-Condição | Elevador funcional | 
| Pós-Condição | Elevador bloqueado até estado de emergência ser desativado | 
| Fluxo Principal | * Botão de emergência do elevador é acionado pelo usuário <br> * Elevador se dirige ao andar mais próximo <br> * Espera até todos os passageiros saírem <br> * Elevador fecha as portas e fica bloqueado até ser liberado | 

<br>

| Identificação | UC - 04 |
|---|---|
| Função | Chamar elevador | 
| Atores | Elevador, Usuário | 
| Prioridade | Essencial | 
| Pré-Condição | Elevador funcional | 
| Pós-Condição | Elevador parado no piso pedido | 
| Fluxo Principal | * Usuário seleciona, através da botoeira, o andar que deseja ir [FS01] <br> * A botoeira indica qual elevador vai leva-lo ao seu andar <br> * O elevador leva o usuário até o andar selecionado  |
| Fluxo Secundário [FS01] | * Andar selecionado está na lista de andares VIP <br> * A botoeira indica qual elevador vai leva-lo ao andar VIP * <br> * O elevador faz o reconheciomento facil do usuário através de sua câmera <br> * O usuário é reconhecido e levado até seu andar[FS02]|
| Fluxo Secundário [FS02] | * O usuário não é identificado pelo reconhecimento facial <br> * O elevador emite um aviso sonoro de falha de reconhecimento e não leva o usuário até o andar solicitado* |

<br>


| Identificação | UC - 05 |
|---|---|
| **Função** | Configurar serviço dos elevadores |
| **Atores** | Elevador, Controlador |
| **Prioridade** | Essencial |
| **Pré-Condição** | Sistema de controle de elevadores ativo e todos os elevadores disponíveis para configuração |
| **Pós-Condição** | Serviço dos elevadores configurado conforme a quantidade de andares e elevadores disponíveis |
| **Fluxo Principal** | * O Controlador acessa o painel de configuração do sistema de elevadores <br> * O Controlador informa o número total de andares e o número de elevadores disponíveis <br> * O sistema calcula automaticamente a distribuição dos andares, de modo que cada elevador (ou conjunto de elevadores) cubra no máximo 12 andares <br> * O sistema apresenta ao controlador a divisão proposta (ex: Elevador 1 → andares 1 a 10; Elevador 2 → andares 11 a 20) <br> * O Controlador confirma a configuração <br> * O sistema salva e ativa a nova configuração de serviço dos elevadores |



## Diagrama de Classes
```mermaid
classDiagram
    class Elevador {
        - id : int
        - andaresCobertos : List~int~
        - estado : String
        - emServico : bool
        + moverPara(andar: int) void
        + abrirPortas() void
        + fecharPortas() void
        + entrarEstadoIncendio() void
        + entrarEmergencia() void
        + realizarReconhecimentoFacial(usuario: Usuario) bool
    }

    class Usuario {
        - id : int
        - nome : String
        - tipo : String  // normal ou VIP
        + solicitarElevador(andar: int) void
    }

    class Recepcionista {
        - id : int
        - nome : String
        + registrarUsuario(dados: Usuario) void
        + registrarUsuarioVIP(dados: Usuario) void
    }

    class Controlador {
        - id : int
        - nome : String
        + autorizarCadastro() bool
        + configurarServico(qtdAndares: int, qtdElevadores: int) void
    }

    class Botoeira {
        - id : int
        - andar : int
        - tipo : String  // interna / externa
        + selecionarAndar(andar: int) void
        + indicarElevador(elevadorId: int) void
    }

    Controlador "1" --> "*" Elevador : configura
    Recepcionista "1" --> "*" Usuario : cadastra
    Botoeira "1" --> "*" Elevador : solicita / indica
    Usuario "1" --> "1" Botoeira : usa
```
## Diagramas de Sequência
### UC_01
```mermaid
sequenceDiagram
    actor Usuario
    participant Recepcionista
    participant Controlador

    Controlador-->>Recepcionista: autorizarCadastro()
    Usuario->>Recepcionista: fornecerInformacoes()
    Recepcionista->>Recepcionista: registrarUsuario(dados)
    Recepcionista-->>Controlador: confirmarCadastro()
    Controlador-->>Usuario: usuarioCadastrado()
    alt [FS01] Cadastro VIP
        Recepcionista->>Recepcionista: registrarUsuario(dados, tipo = "VIP")
        Recepcionista-->>Controlador: confirmarCadastroVIP()
        Controlador-->>Usuario: usuarioCadastradoComoVIP()
    end
```
### UC_02

```mermaid
sequenceDiagram
    participant Elevador

    Elevador->>Elevador: entrarEstadoIncendio()
    Elevador->>Elevador: moverPara(andar = 0)
    Elevador->>Elevador: abrirPortas()
    Elevador-->>Elevador: permanecerNoTerreo()
```

### UC_03
```mermaid
sequenceDiagram
    actor Usuario
    participant Botoeira
    participant Elevador

    Usuario->>Botoeira: pressionarBotaoEmergencia()
    Botoeira->>Elevador: acionarEmergencia()
    Elevador->>Elevador: moverPara(andarMaisProximo)
    Elevador->>Elevador: abrirPortas()
    Elevador-->>Usuario: passageirosSaem()
    Elevador->>Elevador: fecharPortas()
    Elevador-->>Elevador: bloquearAcesso()
```
### UC_04
```mermaid
sequenceDiagram
    actor Usuario
    participant Botoeira
    participant Elevador

    Usuario->>Botoeira: selecionarAndar(andar)
    Botoeira->>Elevador: indicarElevador(elevadorId)
    Elevador->>Elevador: moverPara(andarSelecionado)
    Elevador->>Elevador: abrirPortas()
    Elevador-->>Usuario: transportarParaDestino()
    alt [FS01] Andar é VIP
        Botoeira->>Elevador: indicarElevadorVIP(elevadorId)
        Elevador->>Elevador: realizarReconhecimentoFacial()
        Elevador->>Elevador: moverPara(andarSelecionado)
        Elevador->>Elevador: abrirPortas()
        Elevador-->>Usuario: transportarParaDestino()
    else [FS02] Falha no reconhecimento
        Elevador->>Usuario: emitirAvisoSonoro("Falha no reconhecimento facial")
        Elevador-->>Usuario: acessoNegado()
    end

```
### UC_05
```mermaid
sequenceDiagram
    actor Controlador
    participant Elevador

    Controlador->>Controlador: acessarPainelConfiguracao()
    Controlador->>Controlador: informarDados(qtdAndares, qtdElevadores)
    Controlador->>Elevador: configurarServico(qtdAndares, qtdElevadores)
    Elevador-->>Controlador: apresentarDivisaoProposta()
    Controlador->>Elevador: confirmarConfiguracao()
    Elevador-->>Controlador: configuracaoAtivada()

```
## Diagrama de Estados

### DdE Elevador
<img width="1010" height="385" alt="image" src="https://github.com/user-attachments/assets/16127d98-bdd0-4afa-b2ee-a8fa7dfd2fba" />

## Diagrama de atividades

### Cadastro de usuário
<img width="476" height="231" alt="image" src="https://github.com/user-attachments/assets/a2b6871a-52c9-40a9-8b02-76d255af2f0f" />

### Chamar Elevador
<img width="667" height="541" alt="image" src="https://github.com/user-attachments/assets/8a6a87ea-a967-432c-be4d-bca300ea3d40" />

### Estado de Emergência
<img width="535" height="154" alt="image" src="https://github.com/user-attachments/assets/2ded82c4-a6d6-41bc-9c61-b60194c46f26" />

### Estado de Incêndio
<img width="711" height="80" alt="image" src="https://github.com/user-attachments/assets/7e5df379-13d8-4488-ac9a-acc11742325d" />

### Configurar serviço dos elevadores
<img width="508" height="177" alt="image" src="https://github.com/user-attachments/assets/cd6383a7-b382-46a6-a969-4a801b25f440" />
