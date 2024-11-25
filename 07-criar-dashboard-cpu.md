<h1>
Criar Dashboard no Dynatrace
</h1>

## Passo 1: Criar o Arquivo `HostList.tsx`

- Crie o arquivo `src/app/pages/HostList.tsx` com o seguinte conteúdo:
    ```jsx
    import React from 'react';
    import { Flex } from '@dynatrace/strato-components/layouts';
    import { ProgressCircle } from '@dynatrace/strato-components/content';
    import { IntentButton } from '@dynatrace/strato-components/buttons';
    import { convertToTimeseries } from '@dynatrace/strato-components-preview/conversion-utilities';
    import { TitleBar } from '@dynatrace/strato-components-preview/layouts';
    import { DataTable, TableColumn, TableRow } from '@dynatrace/strato-components-preview/tables';
    import { useDqlQuery } from '@dynatrace-sdk/react-hooks';
    import { CPU_USAGE_QUERY, getHostAvgCpuQuery, getHostCpuUsageQuery } from '../queries';
    import { Colors } from '@dynatrace/strato-design-tokens';
    
    export const HostList = () => {
      const result = useDqlQuery({
        body: {
          query: CPU_USAGE_QUERY,
        },
      });
    
      const columns: TableColumn[] = [
        {
          header: 'Host ID',
          accessor: 'hostId',
          autoWidth: true,
        },
        {
          header: 'HostName',
          accessor: 'hostName',
          autoWidth: true,
        },
        {
          id: 'cpuUsage',
          header: 'CPU Usage',
          columnType: 'meterbar',
          accessor: ({ idle, ioWait, user, system, steal, other }) => [
            { name: 'CPU idle', value: idle },
            { name: 'I/O wait', value: ioWait },
            { name: 'CPU user', value: user },
            { name: 'CPU system', value: system },
            { name: 'CPU steal', value: steal },
            { name: 'CPU other', value: other },
          ],
          config: {
            showTooltip: true,
          },
          ratioWidth: 1,
        },
        {
          id: 'avgCpu',
          header: 'Average CPU %',
          columnType: 'sparkline',
          accessor: (row) => (result.data ? convertToTimeseries([row], result.data.types) : []),
          config: {
            color: Colors.Charts.Rainbow.Magenta.Default,
          },
          ratioWidth: 1,
        },
      ];
    
      return (
        <Flex width="100%" flexDirection="column" justifyContent="center" gap={16}>
          <TitleBar>
            <TitleBar.Title>Hosts Insights</TitleBar.Title>
          </TitleBar>
          {result.isLoading && <ProgressCircle />}
          {result.data && (
            <DataTable data={result.data.records} columns={columns}>
              <DataTable.UserActions>
                <DataTable.RowActions>
                  {(row: TableRow) => (
                    <IntentButton
                      payload={{
                        'dt.elements': [
                          {
                            'dt.markdown': `# Host ${row.values.hostName} insights`,
                          },
                          {
                            'dt.query': getHostCpuUsageQuery(row.values.hostId),
                            'visualization': 'areaChart',
                          },
                          {
                            'dt.query': getHostAvgCpuQuery(row.values.hostId),
                          },
                        ],
                      }}
                    />
                  )}
                </DataTable.RowActions>
              </DataTable.UserActions>
              <DataTable.Pagination />
            </DataTable>
          )}
        </Flex>
      );
    };
    ```
##

##

## Passo 2: Atualizar o Arquivo `App.tsx`

- Atualize o arquivo `src/app/App.tsx` com o seguinte conteúdo:
    ```jsx
    import React from 'react';
    import { Route, Routes, Link } from 'react-router-dom';
    import { Page, AppHeader } from '@dynatrace/strato-components-preview/layouts';
    import { HostList } from './pages/HostList';
    
    export const App = () => {
      return (
        <Page>
          <Page.Header>
            <AppHeader>
              <AppHeader.NavItems>
                <AppHeader.AppNavLink as={Link} to="/" />
              </AppHeader.NavItems>
            </AppHeader>
          </Page.Header>
          <Page.Main>
            <Routes>
              <Route path="/" element={<HostList />} />
            </Routes>
          </Page.Main>
        </Page>
      );
    };
    ```

##

##

### Configurar Métricas

- Atualize o arquivo `app.config.json` com o seguinte conteúdo:
    ```json
    {
      "environmentUrl": "https://rok25300.apps.dynatrace.com",
      "app": {
        "name": "Host Insights",
        "version": "0.0.0",
        "description": "A starting project with routing, fetching data, and charting",
        "id": "my.host.insights",
        "scopes": [
          { "name": "storage:logs:read", "comment": "default template" },
          { "name": "storage:buckets:read", "comment": "default template" },
          { "name": "storage:metrics:read", "comment": "Allows to read metrics" },
          { "name": "storage:entities:read", "comment": "Allows to read entities" }
        ]
      }
    }
    ```

##

##

### Exportar a Consulta DQL

- Atualize o arquivo `src/app/queries.ts` com o seguinte conteúdo:
    ```ts
    export const CPU_USAGE_QUERY = `timeseries cpuAvg = avg(dt.host.cpu.usage), by:{dt.entity.host, host.name}
    | fieldsRename hostId = dt.entity.host, hostName = host.name
    | lookup [
      timeseries {
        idle=avg(dt.host.cpu.idle),
        ioWait=avg(dt.host.cpu.iowait),
        user=avg(dt.host.cpu.user),
        system=avg(dt.host.cpu.system),
        steal=avg(dt.host.cpu.steal)
      },
      by:{dt.entity.host}
      | fieldsAdd idle = arrayAvg(idle), ioWait = arrayAvg(ioWait), user = arrayAvg(user), system = arrayAvg(system), steal = arrayAvg(steal)
      | fieldsAdd other = 100 - idle - ioWait - user - system - steal
    ], sourceField:hostId, lookupField:dt.entity.host, fields:{idle, ioWait, user, system, steal, other}`;
    
    export const getHostCpuUsageQuery = (hostId: string) => `timeseries {
      idle=avg(dt.host.cpu.idle),
      ioWait=avg(dt.host.cpu.iowait),
      user=avg(dt.host.cpu.user),
      system=avg(dt.host.cpu.system),
      steal=avg(dt.host.cpu.steal)
    },
    filter:{dt.entity.host == "${hostId}"}
    | fieldsAdd other = 100 - idle[] - ioWait[] - user[] - system[] - steal[]`;
    
    export const getHostAvgCpuQuery = (hostId: string) =>
      `timeseries cpuAvg = avg(dt.host.cpu.usage), filter:{dt.entity.host == "${hostId}"}`;
    ```

##

##

### Criar o Componente `Card`

- Crie o arquivo `src/app/components/Card.tsx` com o seguinte conteúdo:
    ```jsx
    import React, { useEffect, useState } from "react";
    import axios from "axios";
    import Borders from "@dynatrace/strato-design-tokens/borders";
    import BoxShadows from "@dynatrace/strato-design-tokens/box-shadows";
    import Colors from "@dynatrace/strato-design-tokens/colors";
    import { Flex } from "@dynatrace/strato-components/layouts";
    import { Link } from "@dynatrace/strato-components/typography";
    import { Link as RouterLink } from "react-router-dom";
    
    type CardProps = {
      href: string;
      imgSrc: string;
      name: string;
      inAppLink?: boolean;
    };
    
    const environmentUrl = "https://rok25300.apps.dynatrace.com"; // URL do seu ambiente Dynatrace
    const token = "dt0c01.O55A36M3G6DF7X7JVFBWNN32.DMOXLTHDS4DRJH6YIMRI5PWHA2QOMX2GGUTFL6NUAQS5FWWV7HJC42XVTWRSS3E3"; // Substitua pelo seu token de API
    
    export const Card = ({ href, inAppLink, imgSrc, name }: CardProps) => {
      const [cpuUsage, setCpuUsage] = useState<string>("Carregando...");
    
      // Função para buscar dados de CPU
      useEffect(() => {
        const fetchCpuUsage = async () => {
          try {
            const response = await axios.post(
              `${environmentUrl}/api/v2/metrics/query`,
              {
                query: "builtin:host.cpu.usage", // Consultando a métrica de CPU
                from: "now-15m", // Últimos 15 minutos
                to: "now",
                resolution: "1m", // Resolução de 1 minuto
              },
              {
                headers: {
                  Authorization: `Api-Token ${token}`,
                  "Content-Type": "application/json",
                },
              }
            );
    
            const cpuValue =
              response.data?.result?.[0]?.data?.[0]?.values?.[0]; // Acessando o valor de CPU
    
            setCpuUsage(cpuValue ? `${cpuValue.toFixed(2)}%` : "Dados não disponíveis");
          } catch (error) {
            console.error("Erro ao buscar dados de CPU:", error);
            setCpuUsage("Erro ao buscar dados");
          }
        };
    
        fetchCpuUsage();
      }, []);
    
      const content = (
        <Flex flexDirection="column" alignItems="center" gap={24}>
          <img src={imgSrc} alt={name} height="100px" width="100px" />
          <div>{name}</div>
          <div>{cpuUsage}</div> {/* Exibe o uso de CPU */}
        </Flex>
      );
    
      return (
        <Flex
          flexDirection="column"
          alignItems="center"
          justifyContent="center"
          gap={24}
          style={{
            width: "210px",
            height: "210px",
            textAlign: "center",
            border: `${Colors.Border.Neutral.Default}`,
            borderRadius: `${Borders.Radius.Container.Default}`,
            background: `${Colors.Background.Surface.Default}`,
            boxShadow: `${BoxShadows.Surface.Raised.Rest}`,
          }}
        >
          {/* An in-app link needs to be handled by react-router to avoid full page reloads */}
          {inAppLink ? (
            <Link as={RouterLink} to={href}>
              {content}
            </Link>
          ) : (
            <Link target="_blank" href={href} rel="noopener noreferrer">
              {content}
            </Link>
          )}
        </Flex>
      );
    };
    ```

##

##

### Exemplo de Home

- Crie ou atualize o arquivo `src/app/pages/Home.tsx` com o seguinte conteúdo:
    ```jsx
    import React from "react";
    import { useCurrentTheme } from "@dynatrace/strato-components/core";
    import { Flex } from "@dynatrace/strato-components/layouts";
    import { Heading, Paragraph, Strong } from "@dynatrace/strato-components/typography";
    import { Card } from "../components/Card"; // Importando o Card
    import { HostList } from "./HostList"; // Importando HostList
    
    export const Home = () => {
      const theme = useCurrentTheme();
    
      return (
        <Flex flexDirection="column" alignItems="center" padding={32}>
          <img
            src="./assets/Dynatrace_Logo.svg"
            alt="Dynatrace Logo"
            width={150}
            height={150}
            style={{ paddingBottom: 32 }}
          />
    
          <Heading>my app dynatrace</Heading>
          <Paragraph>
            Edit <Strong>src/app/pages/Home.tsx</Strong> and save to reload the app.
          </Paragraph>
          <Paragraph>
            For more information and help on app development, check out the following:
          </Paragraph>
    
          {/* Componente de Logs */}
          <HostList />
    
          <Flex gap={48} paddingTop={64} flexFlow="wrap">
            {/* Card de CPU - Aqui é onde o uso de CPU será mostrado */}
            <Card
              href="/data"
              inAppLink
              imgSrc={
                theme === "light" ? "./assets/data.png" : "./assets/data_dark.png"
              }
              name="Uso de CPU" // O nome do card
            />
    
            {/* Outros Cards */}
            <Card
              href="https://dt-url.net/developers"
              imgSrc={
                theme === "light"
                  ? "./assets/devportal.png"
                  : "./assets/devportal_dark.png"
              }
              name="Dynatrace Developer"
            />
            <Card
              href="https://dt-url.net/devcommunity"
              imgSrc={
                theme === "light"
                  ? "./assets/community.png"
                  : "./assets/community_dark.png"
              }
              name="Developer Community"
            />
          </Flex>
        </Flex>
      );
    };
    ```

##

##

### Rodar o App

- Execute o comando para iniciar o app (não esqueça de verificar se está no diretório do app):
    ```bash
    npm run start
    ```

- Acesse a aplicação em: [http://localhost:3000/ui](http://localhost:3000/ui)

---

Após seguir esses passos, o servidor de desenvolvimento deve iniciar e abrir o navegador para o ambiente de desenvolvimento do Dynatrace. Boa sorte com o desenvolvimento do seu app!


