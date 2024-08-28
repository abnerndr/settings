# Projeto NestJS

Este repositório contém um projeto básico utilizando o framework [NestJS](https://nestjs.com/). Abaixo estão os passos para iniciar o projeto, instalar as dependências necessárias e configurar as ferramentas principais.

## Pré-requisitos

Certifique-se de ter o [Node.js](https://nodejs.org/) e o [Yarn](https://yarnpkg.com/) instalados em sua máquina.

## Passos para iniciar o projeto

### 1. Criação do Projeto

Primeiro, crie um novo projeto NestJS utilizando o comando abaixo:

```bash
yarn global add @nestjs/cli
nest new nome-do-projeto
```

Substitua nome-do-projeto pelo nome desejado para o seu projeto. Durante a criação, o NestJS perguntará qual gerenciador de pacotes você deseja utilizar. Escolha Yarn.

### 2. Instalação de Dependências

Navegue até o diretório do projeto recém-criado e instale as seguintes dependências:

```bash
cd nome-do-projeto
yarn add @nestjs/config class-validator class-transformer @nestjs/typeorm typeorm pg @nestjs/schedule @nestjs/axios axios
```

## 3. Configurando o Projeto

### 3.1 Configuração de Variáveis de Ambiente

Utilize o pacote *@nestjs/config* para gerenciar as variáveis de ambiente. No diretório src, crie um arquivo config.ts para centralizar as configurações:

```typescript
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: '.env',
    }),
    // outras importações
  ],
  // outras configurações
})
export class AppModule {}
```

Crie um arquivo *.env* na raiz do projeto para armazenar as variáveis de ambiente.

### 3.2 Validação de Dados

Para validação de dados, use os pacotes *class-validator* e *class-transformer*:

```typescript
import { IsString, IsInt } from 'class-validator';

export class CreateUserDto {
  @IsString()
  name: string;

  @IsInt()
  age: number;
}
```

### 3.3 Configuração do TypeORM com PostgreSQL

Configure a conexão com o banco de dados no arquivo *app.module.ts*:

```typescript
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: process.env.DB_HOST,
      port: parseInt(process.env.DB_PORT, 10) || 5432,
      username: process.env.DB_USERNAME,
      password: process.env.DB_PASSWORD,
      database: process.env.DB_NAME,
      entities: [__dirname + '/**/*.entity{.ts,.js}'],
      synchronize: true,
    }),
    // outras importações
  ],
  // outras configurações
})
export class AppModule {}
```

### 3.4 Agendamento de Tarefas

Use o pacote *@nestjs/schedule* para agendar tarefas:

```typescript
import { Injectable } from '@nestjs/common';
import { Cron, CronExpression } from '@nestjs/schedule';

@Injectable()
export class TasksService {
  @Cron(CronExpression.EVERY_10_SECONDS)
  handleCron() {
    console.log('Tarefa executada a cada 10 segundos');
  }
}
```

### 3.5 Requisições HTTP com Axios

Configure o Axios para realizar requisições HTTP:

```typescript
import { HttpService } from '@nestjs/axios';
import { Injectable } from '@nestjs/common';
import { firstValueFrom } from 'rxjs';

@Injectable()
export class ApiService {
  constructor(private readonly httpService: HttpService) {}

  async fetchData(url: string) {
    const response = await firstValueFrom(this.httpService.get(url));
    return response.data;
  }
}
```

## Conclusão

Com essas etapas, você terá um projeto NestJS configurado com as dependências básicas para desenvolvimento. Sinta-se à vontade para expandir conforme a necessidade do seu projeto.

Este `README.md` fornece uma orientação passo a passo para iniciar um projeto NestJS com Yarn e instalar e configurar as dependências mencionadas.

