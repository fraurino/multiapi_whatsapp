# 📲 MultiAPI WhatsApp

Integração com WhatsApp utilizando múltiplas APIs no mesmo projeto.

---

## 📢 Sobre o Projeto

💬 Este é um projeto demonstrativo para **envio de mensagens via WhatsApp** utilizando diferentes APIs:

- ✅ WPPConnect  
- ✅ Evolution API  
- ✅ Waha  
- ✅ Whapi  

🔧 Desenvolvido em **Delphi (Pascal)** com foco em modularidade:

- Arquivos separados por API;
- Classe integradora central (`TWhatsAppManager`);
- Exemplo funcional com validação automática de recursos por API.

> 🎯 Ideal para quem deseja aprender ou implementar integrações reais com WhatsApp via API!
---
> Demo para testes : 🔗 [Clique aqui para fazer o download](https://github.com/fraurino/multiapi_whatsapp/blob/main/MultAPIWhatsapp.exe)
> 🔗 [Resultado do Teste vírus total](https://www.virustotal.com/gui/file/a1233e80c8b14b58c570be60bcc7c4e69487ae36880d92cd566e863cea0d6a68?nocache=1)



---

## 🧰 Recursos Implementados

- 📷 Autenticação por QR Code (imagem ou código);
- 💬 Envio de mensagens simples;
- 📎 Envio de anexos;
- ⌨️ Simulação de digitação;
- 📬 Status da mensagem (enviada, recebida, lida);
- 📂 Arquivar/desarquivar conversas;
- 🚫 Bloquear/desbloquear contatos;
- 🧑‍💼 Perfil com imagem;
- 🆔 Gerenciamento de instâncias e status;
- 🧠 Classe `Manager` centralizando todas as chamadas às APIs.

> ⚠ *Recursos disponíveis conforme suporte de cada API.*

---

## 📸 Exemplo Visual

![Exemplo](https://github.com/user-attachments/assets/29ade204-7459-48ac-a685-f0c80dd7db19)

---

## ⚙ Tecnologias Utilizadas

- Delphi (Pascal)
- APIs REST com requisição por *HttpClient* 

---

## 🤝 Entre no Grupo

Conheça mais sobre o projeto e junte-se ao grupo exclusivo:

🔗 [Acesse o grupo no WhatsApp](https://chat.whatsapp.com/I7mCbnJuRHnBJ3JQU1hDQF)

---

## 🧪 Exemplo de Uso

### 🔸 Enviando uma mensagem com simulação de digitação:

```delphi

// funcao enviar mensagem....
if ckDigitando.Checked then
  zap.SimularDigitacao(numero);

// Enviar mensagem com anexo
if zap.EnviarMensagem(numero, mensagem) then
begin
  // Capturar retorno da mensagem
  zap.RetornoEnvioMensagem;

  if not zap.instanceId.IsEmpty then
    mmRetorno.Lines.Add('instanceId: ' + zap.instanceId);

  // ID da mensagem para consulta futura
  idmsg.Text := zap.Id;
end;
//classe manager gerenciando o uso da api selecionada pela aplicação 
function TWhatsAppManager.EnviarMensagem(Numero, Mensagem: string): Boolean;
var
  DummySent: Boolean;
  DummyID, DummyStatus: string;
begin
  Result := False;
  mensagem := TratarMensagem(mensagem); // Trata caracteres especiais, etc.

  case FTipoAPI of
    apiWahaResult:   Result := FWaha.SendMessage(Numero, Mensagem);
    apiEvolution:    Result := FEvo.SendMessage(Numero, Mensagem);
    apiWppConnect:   Result := FWpp.SendMessage(Numero, Mensagem);
    apiWhapiCloud:
      Result := FWhapi.SendTextMessage(
        FWhapi.FToken,
        Numero,
        Mensagem,
        0,      // TypingTime
        0,      // EphemeralTime
        False,  // NoLinkPreview
        False,  // ViewOnce
        DummySent,
        DummyID,
        DummyStatus
      );
  end;
end;
