## Configurando ambiente de desenvolvimento

-   Inicializar a aplicação

    -   npm run dev

-   Extensão

    -   GraphQL
    -   Tailwind CSS IntelliSense
    -   PostCSS Language Support

-   Instalação vite (modelo ecmascript model que transforma a tecnologia em nativa para nabegador)
-   npm create vite@latest

    -   framework react
        -   react-ts (type script)

-   Instalação tailwindcss (bibliotaca de CSS que ajuda a uma maior performance na estilização), postcss(ferramenta qu o vite utiliza por baixo, que serve para automatilar tarefas dentro do css), e autoprefixer

    -   npm i tailwindcss postcss autoprefixer -D

-   Tailwindcss (-p para crias arquivo de configuração do post css)

    -   npx tailwindcss init -p

-   Configurar a pasta tailwind.conf.js com as especificações dos arquivos que serão utilizados, dentro do "content" do arquivo
    -   content: ["./src/**/*.tsx"], (arquivos dentro de src/todos os arquivos/todos que tiverem a extensão tsx, serão utilizados pelo tailwindcss)

## CMS = Content Management System (Gerenciador de conteúdo rotativo, para que o cliente possa inserir dados)

Programas como worldpress não são headless e trazem tanto o painel de ADMIN quanto a parte visual do front-end (temas), back end e front end sendo o mesmo sistema

//Headless CMS : tráz apenas o painel de ADMIN (dados fornecidos através de uma API, que pode ser REST ou GraphQL)
// React que consome essa API do CMS
Dando muito mais flexibilidade, usando a biblioteca ou tema que desejar, mais atrativa e amigável para o usuário

https://app.graphcms.com/

No GraphQL existem 2 tipos de operações principais (query ou mutation)
\\ query = buscar dados
\\ mutation = criar, alterar ou deletar dados

No GraphCMS, na aba playground é possível fazer as requisições das requisições clicando nos conteúdos que vc quer acessar ou pela sintaxe
ex:
query MyQuery {
lesson {
id
slug
title
teacher {
name
bio
avatarURL
}
}
}

## Iniciando App

-   Deleta conteúdo do App np return da função

-   Cria pasta style e seta entradas

    -   @tailwind base @tailwind components @tailwind utilities; (3 partes que serão utilizadas na estilização)

-   Importa no main o arquivo novo (global.css) que será utilizado

    -   import './styles/global.css'

-   Altera com as extensões do tailwind o App.

    -   className="text-2xl" (altera tamanho da fonte pra 24px, que por baixo já faz uso de medidas relativas (REM)), quanto maior o número, maior a proporção do tamanho da fonte

-   Estilizando através de classes (sempre colocar @apply se for estilizar no global.css)

    -   negrito (font-bold)
    -   cor de fonte (text-"violet"-500)
    -   cor de fundo (bg-zin-900 (cinza escuro))
    -   cor de texto (tex-zinc-100 (cinza claro))

## Consumindo a URL

-   Content API do projeto no graphicms
-   Fazer uma chamada com a ferramenta Apollo (feita pra requisição GraphQL)

-   Instalação do Apollo client com o graphQl

    -   npm i @apollo/client graphql

-   Criar uma pasta no src chamada "lib"

    -   Criar uma pasta na lib chamada "apollo.ts" (arquivo de configuração do apollo)
    -   export const client = new ApolloClient({
        uri:'endereço URL',
        cache: new InMemoryCache()
        })

-   Fazer requisição (com Apollo) no App.tsx

    -   Uma opção é:

    const GET_LESSONS_QUERY = gql`query { lessons { id title } }`
    function App() {
    useEffect(()=>{
    client.query({
    query: GET_LESSONS_QUERY,
    }).then(response => {
    console.log(response.data);

        })

    },[])
    return (
    <h1 className="text-2xl" >Hello World</h1>
    )
    }

-   Opção useQuery (hook (funções que adicionam algum comportamento ao componente (buscar dados de uma API)) do react)

    -   Para isso precisamos ir ao main.tsx e indicar o <ApolloProvider>

    function App() {
    const { data } = useQuery(GET_LESSONS_QUERY)

    console.log(data);

    return (
    <h1 className="text-2xl" >Hello World</h1>
    )
    }

-   Mostrar em tela, no App.tsx indicar dentro do return

    -   Buscando com um map a Lesson dentro de lessons

    const GET_LESSONS_QUERY = gql`query { lessons { id title } }`
    interface Lesson {
    id:string;
    title: string
    }

function App() {
const { data } = useQuery <{ lessons: Lesson[]}> (GET_LESSONS_QUERY)

console.log(data);

return (

<ul>
{data?.lessons.map(lesson => {
return <li>{lesson.title}</li>
})}
</ul>
)
}

## Dividindo e estilizando os componentes

-   Criar pasta componets em SRC

-   Para não gerar errors, criar alguma informação dentro de cada uma delas

-   Como serão 2 páginas, criar uma pasta pages, com as páginas estruturadas

-   Importar a página do pages no App.tsx

-   Coverter copy do SVG para jsx
    -   https://svg2jsx.com/ (retorna um componente do react)
    -   Copia o código react em um pasta específica para ele (Logo.tsx)

## Configurações de estilo

-   Para imagem ocupar tamanho tode de tela (className='w-full' )

-   Padding p(eixo x ou y)-tamanho de padding (multiplos de 4)

    -   (className = 'py-5) significa que temos 20px de padding em cima e embaixo

-   Padrão flex

    -   flex items-center justify-center

-   Customizando cores no tailwind
-   No tailewind.config.js existe o theme, e dentro dele o extend. Lá é possive estender as cores do tailwind (agora é possível a utilização dessa cor em toda a aplicação)

-   borda (border-b) o b significa botton de 1px

-   cor da borda (border-color) ex.border-dray-600

-   para aplicar setagens gerais

    -   styles/global
    -   ex. body {
        @apply bg-gray-900 text-gray-100;
        }

-   Para capturar um fonte, ir para Google fonts
    -   copiar o link
    -   copiar o link no head do index.html
    -   Dizer ao tailwind que vamos usar essa fonte por padrão em todos os textos
        -   tailwind.config.js passar antes do color o font-family
        -   ex. fontFamily: {
            sans: "Roboto, sans-serif",
            },

\*Customizando os componentes

-   Chamar Sidebar entro de Event.tsx
-   Envelopar ela em main (toda a tela, sem Header)
-   Incluir o Video na main
-   Indica a propriedade flex no main
-   No Video.tsx indicar flex-1 para esticar ou reduzir conforme for preciso
-   Envelopar o Sidebar no aside com uma medida que o Ctrl ajuda a indicar por causa dos padrões de tamanho, se não houver um tamnho fixo é indicado por []
    -   e. w-[348px] bg-gray-700 p-6 border-l border-gray-600 (width, background-color, padding, border left, cor da borda)
-   Para ocupar todo a altura da tela (min-h-screen)
-   Criar os componentes de dentro do sidebar
-   Para criar um efeito mais suave na fonte, especialmente em Mac(-webkit-font-smoothing: antialiased;)
-   Criamos as eareas de Lesson na Sidebar
-   Estilização da div Lesson
    -   flex flex-col gap-8 (direction collun e gap de 8x4=32px)
-   Criar o html da Lesson.tsx com a âncora para linkar na aula
-   Acrescentar o CSS nas divs (rounded = radius de 4px) (border 1px de todos os lados) (p- padding (4 = 16px de todos os lados)) (mt = margin-top) (text-sn define tamanho da fonte em 14px) (text-xs fonte 12px) (py-[2px] px-2 = em cima e embaixo em 2px e laterais em 8px)

## Adicionando uma biblioteca de Ícones

-   npm i phosphor-react

-   Importa em Lesson.tsx
    -   import {CheckCircle} from "phosphor-react"
-   Usa como uma tag

## Deixando componente dinâmico (Lesson.tsx)

Dinamismo para variae dependendo da aula, liberada ou não, ao vivo ou não

-   Criar uma porta para modificar componente
-   Criar uma interface indicando quais conponentes serão dinâmicos

-Indica como parâmetro o Props dos componentes

-   Muda os componentes estáticos pelo props.atributo = {props.title} {props.availableAt.toString()}

-   Cria a condicional no AO VIVO

    -   {props.type === 'live' ? 'AO VIVO' : 'AULA PRÁTICA'}

-   Cria uma condicional para todo o conteúdo liverado
    -   uma const auxiliar e temporária para indicar se está avalable ou não
    -   Dentro da condicional todas as linhas de códgo com o estilo em cada condição Lessonavailable

## Enviando as propriedades criadas

-   Em sidebar, dentro da tag criada (Lesson), envia os conteúdos

-   Buscar as aulas dentro do GraphCMS

    -   Criar uma const chamando gql com oq vai ser ofertado (copiar a requisição do GraphCMS)
    -   Cria uma const {data} = useQuery para usar informações

-   Interface pra passar o tipagem do retorno

-   retorna a interface dentro da const

-   Percorre todas as lessons e devolve os conteúdos indicados
    ex. <div className="flex flex-col gap-8">
    {data?.lessons.map(lesson => {
    return(
    <Lesson
    title="Aula1"
    slug="aula-01"
    availableAt={new Date()}
    type="class"
    />
    )
    })
    }

-   Quando percorremos um array e retorna um elemento, no react temos que acrescentar o Key = {"informaçnao unica que temos em cada item da lista}

## Instalando API (biblioteca) de datas

-   npm i date-fns

-   Importa em Lesson.tsx a primeira função da biblioteca

    -   isPast que é uma função que verifica se uma data já passou

-   Na const isLessonAvailable chama a função isPast com a propriedade props.availableAt

## Criando uma nova aula

-   Com data que está por vir

-   Cria a aula no graphsCMS e publica

## Formatando o texto da data

-   Usar o método "format" que está dentro da biblioteca date-fns

-   Chamar no import do Lesson.tsx

-   Cria uma constante para formatação (const availableDateFormated)

-   Primeiro parâmetro é a data props.availableAt e o segundo o formato de data que será usado

    -   Aspas duplas no formato e os caracteres que estão na documentação do FNS, onde caracteres irão representar o conteúdo da data. As aspas simples serão textos que não vão ser formatados
        -   EEEE (dia da semana), ' ' com o ponto dentro •, dia (d) ' de ', MMMM (mês), ' ' com o ponto dentro •, k (hora), ' h ', mm (minuto),

-   Para converter de ingles para Portugues

    -   importar ptBR da localização "date-fns/locale/pt/pt-BR"
    -   Passa como terceiro parâmetro no formato formated

    ## Estilizando vídeo

-   Na div de conteúdo do vídeo h-full w-full max-w-[1100px] max-h-[60vh] (largura e altura tela inteira, max largura e max altura)
    -   aspect-video(propriedade do aspect-ratio q é a proporção)
-   "p-8 max-w[1100px] mx-auto" (padding 32px, largira máxima. margin laterais)

-   Logos estão no pacote de ícones. ex. <DiscordLogo />

-   Efeitos de hover = passar com : e tudo que vier depois é aplicado ex. hover:bg-blue-500 |hover:text-gray-900

-   Transições passadas diretamente. ex. transition-colors

-   leading-relaxed = line-height:162,5%

-   grid-cols-2 cria duas colunas com as memas setagens

-   overflow-hidden todos os elementos internos nunca vão poder passar com container

-   items-stretch se estica para cumprir a maior altura possivel dentro do container

## Instalando biblioteca vimeJs (vídeos do site)

-   https://vimejs.com/
    -   Instalation
        -   React
        -   acessa pacotes de Instalação
        -   npm i @vime/core @vime/react --force (talvez mude, olhar documentação) (tive que forçar instalação)
