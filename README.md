## 3. Cadastro: Evitar cliente/fornecedor duplicado via CNPJ

### 🛠 Problema real
No Protheus, é comum usuários cadastrarem **clientes ou fornecedores duplicados** por não consultarem o sistema corretamente ou por falta de validações automáticas. O campo CNPJ (CGC) não é único por padrão, e isso leva à criação de múltiplos cadastros com o **mesmo documento**, o que causa confusão em pedidos, pagamentos, e relatórios financeiros.

### 📉 Impacto
- Confusão em pedidos de compra/venda
- Contas a pagar ou receber duplicadas
- Relatórios inconsistentes
- Risco de faturamento e pagamentos incorretos
- Dificuldade na conciliação contábil e fiscal

### 💡 Solução aplicada
Criação de uma rotina de **validação automática no momento do cadastro** (ou importação) para verificar se já existe um cliente ou fornecedor com o mesmo CNPJ (campo `A1_CGC` ou `A2_CGC`).

Caso já exista, o sistema **bloqueia o cadastro** e informa o usuário com uma mensagem clara.

### 🧾 Código exemplo (ADVPL)
```advpl
User Function ValidaCNPJ(cCNPJ, cTipo)
    Local cTabela := If(cTipo == "C", "SA1", "SA2")
    Local cCampo  := If(cTipo == "C", "A1_CGC", "A2_CGC")

    dbSelectArea(cTabela)
    dbSetOrder(3) // Índice por CNPJ
    If dbSeek(xFilial(cTabela) + cCNPJ)
        MsgStop("Já existe um cadastro com este CNPJ.")
        Return .F.
    EndIf

Return .T.
```
## 🧪 Testes realizados

| Situação                          | Resultado Esperado                          |
|---------------------------------------|-----------------------------------------|
| Cadastro com CNPJ novo                | ✅ Cadastro permitido                   |
| Cadastro com CNPJ existente           | ❌ Bloqueado, com aviso                 |
| Importação via Excel com CNPJ duplicado | ❌ Registro ignorado ou logado        |

🎯 Benefícios
Evita duplicidade de cadastros no SA1/SA2

Garante consistência nos processos comerciais e financeiros

Reduz retrabalho da equipe

Melhora a qualidade dos dados mestres da empresa

🏷️ Tags
#Protheus #ADVPL #Cadastro #CNPJ #Validação #SA1 #SA2 #Backoffice
