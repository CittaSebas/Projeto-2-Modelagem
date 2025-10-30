# Projeto-2-Modelagem
UML de um sistema para  o controle de elevadores em uma rede de hotéis

## Diagrama de Atores 
<img width="644" height="517" alt="image" src="https://github.com/user-attachments/assets/c7b95549-65b9-4b06-8461-26f22147f7e9" />


## Casos de Uso

| Identificação | UC - 01 |
|---|---|
| Função | Verificar VIP | 
| Atores | Elevador | 
| Prioridade | Importante | 
| Pré-Condição | Registro dos hóspedes completo e câmera funcional no elevador | 
| Pós-Condição | Andar desbloqueado para o usuário | 
| Fluxo Principal | * O hóspede aperta o botão do andar VIP <br> * A câmera detecta a face do hóspede e envia as informações ao sistema de controle <br> * [FS01] O sistema compara com o registro interno de hóspedes VIP e libera o elevador para uso VIP <br> | 
| Fluxo Secundário [FS01] | * O sistema não encontra a face na base de dados <br> * O elevador envia um alerta sonoro para alertar o usuário |

<br>

| Identificação | UC - 02 |
|---|---|
| Função | Ativar estado de incêndio | 
| Atores | Elevador | 
| Prioridade | Essencial | 
| Pré-Condição | Elevador funcional | 
| Pós-Condição | Elevador bloqueado até incêndio finalizar | 
| Fluxo Principal | * O sensor de fumaça do predio capta fumaça <br> * Predio entra em estado de incêndio <br> * Elevador se dirige ao 1º andar <br> * Elevador é bloqueado | 

<br>

| Identificação | UC - 03 |
|---|---|
| Função | Ativar estado de emergência | 
| Atores | Elevador, Usuário | 
| Prioridade | Essencial | 
| Pré-Condição | Elevador funcional | 
| Pós-Condição | Elevador parado | 
| Fluxo Principal | * Botão de emergência do Elevador é acionado pelo usuário <br> * Elevador se dirige ao andar mais próximo | 

<br>

| Identificação | UC - 04 |
|---|---|
| Função | Chamar elevador | 
| Atores | Elevador, Usuário, Controlador | 
| Prioridade | Essencial | 
| Pré-Condição | Elevador funcional | 
| Pós-Condição | Elevador parado no piso pedido | 
| Fluxo Principal | * Botão de chamada é acionado por Usuário ou Controlador <br> * Elevador se dirige ao andar no qual foi chamado | 

<br>

| Identificação | UC - 05 |
|---|---|
| Função | Registrar usuário | 
| Atores | Usuário, Controlador | 
| Prioridade | Essencial | 
| Pré-Condição | Sistema funcional | 
| Pós-Condição | Usuário cadastrado | 
| Fluxo Principal | * Usuário cede informações ao controlador <br> * Controlador registra usuário como usuário normal ou VIP | 

<br>

| Identificação | UC - 06 |
|---|---|
| Função | Configurar serviço dos elevadores | 
| Atores | Elevador, Controlador | 
| Prioridade | Essencial | 
| Pré-Condição |  | 
| Pós-Condição | Serviço dos elevadores configurados | 
| Fluxo Principal | * Usuário cede informações ao controlador <br> * Controlador registra usuário como usuário normal ou VIP | 


## Diagrama de Classes
<img width="818" height="551" alt="image" src="https://github.com/user-attachments/assets/97f2d05d-fc50-43a2-b863-0f72ea0081c0" />

