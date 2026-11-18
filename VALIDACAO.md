# Validação — 04/10/2026

- Thoth local em `http://localhost:8000`, Chrome 154 e Selenium 4.50.0.
- Compilação Maven com alvo Java 17, executada no Java 26.
- Execução padrão: **12/12 casos de Informações Gerais aprovados**.
- Execução com `-Dsuite=all`: **32/32 verificações aprovadas**, zero falhas.
- Cada execução criou sua própria conta e um projeto novo. Os projetos usados
  nas duas execuções aprovadas foram 11 e 25; esses IDs não aparecem na configuração.
- A execução completa partiu de outra pasta usando `mvn -f .../pom.xml`, com
  `-DbaseUrl=http://localhost:8000/`, confirmando a leitura dos CSVs pelo classpath
  e a normalização da barra final da URL.
- Não foram testadas outras portas ou instalações da Thoth; a URL é configurável
  e depende de a aplicação estar disponível e configurada nesse endereço.

Uma execução intermediária teve um timeout isolado ao abrir o planejamento no
CT-IG-006. A repetição completa passou. O teste continua reprovando se a página
não carregar e agora informa a URL e o texto da tela para facilitar o diagnóstico;
não há repetição automática que transforme uma falha em aprovação.

Os 12 IDs dos CSVs foram conferidos. As classes anteriores de login e projeto
foram preservadas. O pacote não inclui credenciais reais, IDs fixos, bibliotecas
baixadas, arquivos compilados nem caminhos pessoais da máquina usada na validação.
