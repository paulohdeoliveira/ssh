#### Configuração 2FA U2F

>[!IMPORTANT]
> É necessário conectar a chave de segurança física antes de gerar as chaves.

>[!TIP]
> U2F é a melhor opção disponível para autenticação de dois fatores

Gerar Chaves
```
ssh-keygen -t ed25519-sk
```

Copiar chave pública para o servidor
```
ssh-copy-id -i .ssh/id_ed25519_sk.pub user@aa.dd.rr.es
```



#### Configuração 2FA OTP

Instalar google authenticator
```
sudo apt install libpam-google-authenticator
```
Configurar o google authenticator

```
google-authenticator
```

##### Arquivo /etc/ssh/sshd_config

>[!NOTE]
> Para ativar o OTP é necessária a configuração das linhas abaixo
```
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication yes
AuthenticationMethods publickey,keyboard-interactive
UsePAM yes
```

##### Arquivo /etc/pam.d/sshd

Colocar no início do arquivo

```
#@include common-auth
auth required pam_google_authenticator.so
```
>[!NOTE]
> O parâmetro ```@include common-auth``` precisa apenas ser comentado pois já existe no arquivo

>[!IMPORTANT]
> A configuração do google authenticator é realizada por usuário.
> Para permitir login de usuários que ainda não configuraram o OTP substituir ```auth required pam_google_authenticator.so``` por ``` auth required pam_google_authenticator.so nullok```
>
> Para que o código que está sendo digitado seja mostrado  ```auth required pam_google_authenticator.so echo_verification_code```
