# 📊 Diagramas UML do Sistema

## Visão Geral do Sistema
## Diagrama de Caso de Uso
```plantuml

@startuml
left to right direction

actor "Usuário" as Usuario
actor "Administrador" as Admin
actor "Sistema de Geolocalização" as Geo

rectangle "ExploraAI" {
  usecase "Fazer login"
  usecase "Cadastrar-se"
  usecase "Atualizar preferências"
  usecase "Selecionar cidade manual"
  usecase "Solicitar recomendações"
  usecase "Visualizar lista de locais"
  usecase "Selecionar local"
  usecase "Avaliar local"
  usecase "Salvar como favorito"
  usecase "Cadastrar novo local"
  usecase "Editar local"
}

Usuario --> (Fazer login)
Usuario --> (Cadastrar-se)
Usuario --> (Atualizar preferências)
Usuario --> (Selecionar cidade manual)
Usuario --> (Solicitar recomendações)
Usuario --> (Visualizar lista de locais)
Usuario --> (Selecionar local)
Usuario --> (Avaliar local)
Usuario --> (Salvar como favorito)

Admin --> (Cadastrar novo local)
Admin --> (Editar local)

Geo --> (Solicitar recomendações)

@enduml

```

## Casos de Uso Descritivo
### CSU01 – Solicitar Recomendações

*Finalidade:*  
Gerar uma lista de recomendações personalizadas de locais (turismo, hotéis, restaurantes) com base nas preferências e localização do usuário.

*Atores:*  
Usuário, SistemaGeolocalizacao

*RF:*  
- RF01: Capturar localização atual
- RF02: Permitir que o usuário escolha uma cidade alternativa
- RF03: Consultar locais com base nas preferências

*RNF:*  
- RNF01: Resposta em até 3 segundos
- RNF02: Utilizar dados criptografados
- RNF03: Interface responsiva e acessível

*Pré-condição:*  
Usuário logado e com preferências cadastradas.

*Pós-condição:*  
Sistema exibe uma lista personalizada de locais.

*Fluxo Principal:*  
1. Usuário solicita recomendações.  
2. O sistema captura a localização ou aceita uma cidade alternativa.  
3. O sistema busca locais compatíveis com o perfil.  
4. Exibe a lista de resultados.  

*Fluxo Alternativo:*  
- FA01: Se não houver localização válida, solicitar manualmente.  
- FA02: Se não houver locais, exibir aviso amigável.

---

### CSU02 – Avaliar Local

*Finalidade:*  
Permitir que o usuário avalie um local após sua experiência.

*Atores:*  
Usuário

*RF:*  
- RF04: Inserir nota de 1 a 5
- RF05: Inserir comentário

*Pré-condição:*  
Usuário logado e local visitado.

*Pós-condição:*  
Avaliação salva com sucesso.

*Fluxo Principal:*  
1. Usuário acessa detalhes do local.  
2. Seleciona a opção "Avaliar".  
3. Informa nota e comentário.  
4. Sistema salva a avaliação.

*Fluxo Alternativo:*  
- FA01: Se o usuário já avaliou o local, o sistema exibe um aviso.
-



## 🔹 Diagrama de Classes

```plantuml

@startuml

class Usuario {
  +id: Int
  +nome: String
  +email: String
  +senha: String
  +login(): Boolean
  +atualizarPreferencias(): void
}

class Preferencia {
  +id: Int
}

class PreferenciaAlimentar {
  +vegetariano: Boolean
  +vegano: Boolean
  +restricaoGluten: Boolean
  +tipoCulinaria: String
}

class PreferenciaTurismo {
  +gostaPraia: Boolean
  +gostaMontanha: Boolean
  +gostaFesta: Boolean
  +gostaCultura: Boolean
}

class PreferenciaHospedagem {
  +orcamentoDiaria: Float
  +tipoHospedagem: String
  +nivelConforto: String
}

class Local {
  +id: Int
  +nome: String
  +tipo: String
  +descricao: String
  +localizacao: String
  +faixaPreco: String
  +mediaAvaliacao: Float
}

class Avaliacao {
  +id: Int
  +nota: Int
  +comentario: String
  +data: Date
}

class Favorito {
  +id: Int
  +dataSalvo: Date
}

class Recomendador {
  +gerarRecomendacoes(u: Usuario, tipo: String): List<Local>
}

class Geolocalizacao {
  +capturarPosicao(): String
}


Usuario "1" -- "1" Preferencia
Preferencia "1" -- "1" PreferenciaAlimentar
Preferencia "1" -- "1" PreferenciaTurismo
Preferencia "1" -- "1" PreferenciaHospedagem

Usuario "1" -- "*" Avaliacao
Usuario "1" -- "*" Favorito

Local "1" -- "*" Avaliacao
Local "1" -- "*" Favorito

Avaliacao "1" -- "1" Usuario
Avaliacao "1" -- "1" Local

Favorito "1" -- "1" Usuario
Favorito "1" -- "1" Local

Recomendador ..> Usuario
Recomendador ..> Local
Recomendador ..> Geolocalizacao

@enduml

```


## 🔹 Diagrama de Estados

```plantuml
@startuml
[*] --> Disponivel

Disponivel --> Visualizado : usuário seleciona na lista
Visualizado --> Favoritado : usuário clica em "Salvar como favorito"
Visualizado --> Avaliado : usuário envia avaliação
Visualizado --> Ignorado : usuário volta sem ação
Favoritado --> Desfavoritado : usuário remove dos favoritos
Avaliado --> Reavaliado : nova avaliação enviada
Ignorado --> Disponível : retorno à lista

@enduml

```
