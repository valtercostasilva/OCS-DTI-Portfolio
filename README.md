# Sistema de Apoio à Decisão para Operações no Mercado Financeiro

**Projeto pessoal de desenvolvimento de software | 2026 | Em produção e evolução contínua**

Sistema de apoio ao planejamento e ao acompanhamento de operações no mercado financeiro. A solução integra alertas de indicadores do TradingView a três módulos em Python: **DTI, OCS Market e OCS Sequence**. Juntos, eles realizam o planejamento, relacionam o plano vigente aos eventos observados e comunicam informações operacionais pelo Telegram.

**A decisão e a execução das operações permanecem sob responsabilidade do operador.**

**[📄 Ver o portfólio completo (PDF)](portfolio/Portfolio_Valter.pdf)** — contexto, evolução do projeto, arquitetura, caso real e aprendizados de engenharia de software.

## Problema e objetivo

Antes da automação, a preparação dos gráficos e a análise manual do mercado consumiam horas de atenção.

Atualmente, a preparação dos gráficos leva cerca de quatro minutos. O DTI realiza a análise e apresenta o planejamento para leitura e validação do operador.

O objetivo do projeto é reduzir tarefas repetitivas e organizar as informações necessárias ao planejamento, à tomada de decisão e ao acompanhamento operacional.

## Arquitetura: DTI e dois módulos OCS

O sistema possui **três módulos com responsabilidades distintas**:

| Módulo                     | Responsabilidade                                                                                                                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **DTI — Daily Trade Idea** | Recebe informações dos indicadores, realiza o planejamento diário e produz o plano vigente, incluindo os comportamentos de mercado esperados e os alvos calculados durante o planejamento. |
| **OCS Market**             | Recebe o plano do DTI e avalia qual cenário está vigente conforme o planejamento definido pelo DTI.                                                                                                     |
| **OCS Sequence**           | Recebe webhooks com eventos e snapshots do mercado, acompanha as etapas da sequência operacional e informa o estado atual e o próximo estágio esperado.                                    |

**Fluxo resumido:**

```text
TradingView / indicadores Pine Script
                 |
          Webhooks HTTP/JSON
                 |
        +--------+-----------+
        |                    |
       DTI              OCS Sequence
        |               (eventos, etapas,
   Plano vigente          próximo estágio)
        |                    |
    OCS Market               |
 (plano × mercado,           |
  cenário vigente)           |
        |                    |
        +-- Estado / MySQL --+
                     |
              Saídas no Telegram:
       DTI | OCS Market | OCS Sequence
```

O diagrama apresenta uma **visão conceitual das responsabilidades e dos fluxos**. A representação visual da arquitetura está na página 4 do portfólio.

## Tecnologias e práticas

* **Pine Script e TradingView:** indicadores e alertas de entrada.
* **HTTP, JSON, APIs REST e webhooks:** comunicação entre os componentes.
* **Python e Flask:** módulos de processamento e recebimento de eventos.
* **MySQL e estado operacional:** dados operacionais, histórico e acompanhamento do plano e da sequência.
* **Telegram:** apresentação dos resultados do planejamento e do acompanhamento operacional.
* **Práticas de engenharia:** validação de payloads, contrato versionado, controle de duplicidade, persistência e recuperação de estado, separação de responsabilidades e revisão de comportamentos observados em uso real.

## Caso real documentado

O portfólio apresenta um caso de **31/08/2026**, no qual o DTI calculou os alvos durante o planejamento, antes da conclusão da sequência operacional.

O **OCS Market** relacionou o plano vigente ao cenário observado, enquanto o **OCS Sequence** acompanhou a evolução das etapas e informou o próximo estágio esperado.

As capturas de tela e a linha do tempo estão na **página 5 do PDF**.

## Desenvolvimento e autoria

O conhecimento de mercado, os objetivos do sistema e as regras de negócio foram definidos por **Valter Costa Silva**.

Ferramentas de IA foram utilizadas como apoio à estruturação, implementação, revisão e depuração do software. O código gerado é revisado, validado e, quando necessário, ajustado antes da execução.

O projeto segue em desenvolvimento e também serve como ambiente de aprendizado em engenharia de software.

## Sobre este repositório

Este repositório disponibiliza a **documentação de apresentação do projeto**. O código-fonte dos módulos operacionais não faz parte desta publicação.

O portfólio poderá receber novos casos e atualizações sem alterar o endereço do repositório.
