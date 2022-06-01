[TOC]

# Sintaxes e tutoriais - Java Mobile

## Criar Toast

```java
Toast.makeText(MainActivity.this, "Mensagem aqui", Toast.LENGTH_LONG).show();
// Parâmetros: contexto, mensagem, duração
```



## Atribuindo uma View como variável

```java
// nomeDaView = (TipoDaView) encontrarViewPeloID;
edtNome = (EditText) findViewById(R.id.editTextPersonName);
// (TipoVariável) nomeVariável = nomeDaView.(funções para pegar e converter dados)
String nome = edtNome.getText().toString();
```



## Navegar entre Activities (e enviar dados via Bundle)

### Na Activity origem

```java
// IR E ENVIAR DADOS PARA OUTRA ACTIVITY
// Criar uma intenção  |  Parâmetros: (contexto, destino)
Intent goCatalogActivity = new Intent(getApplicationContext(), Catalogo.class);
// Enviar informações
Bundle cadastro = new Bundle();    // Um Bundle é um dado tipo "Chave: Valor" (parece o dicionário de python)
cadastro.putString("kNome", nome);
cadastro.putString("kEndereco", endereco);
goCatalogActivity.putExtras(cadastro);  // Aqui estou adicionando o Bundle cadastro na Intent goCatalogActivity
startActivity(goCatalogActivity);  // iniciei a Intent e realizei a mudança de tela
```

### Na Activity destino

```java
// RECEBER DADOS DA ACTIVITY ORIGEM
Intent intentReceberCadastro = getIntent();  // Criar uma Intent que irá receber a que foi enviada
Bundle dadosDoPedido = intentReceberCadastro.getExtras();  // Criar um Bundle que irá receber o que foi enviado
// o bundle irá receber os dados extras da intent

String nome = dadosDoPedido.getString("kNome");
String endereco = dadosDoPedido.getString("kEndereco");
// aqui eu atribui os dados do bundle para variáveis String. Mas eu poderia acessar os dados através das chaves do bundle.
```



## Catálogo

Clicar em botão e gerar um scrool para uma view específica

### Dentro da classe

```java
// Atribuir a uma variável tipo Button uma view
Button goToPaos = (Button) findViewById(R.id.btnGoToPao);
// Atribuir a uma variável a ScrollView do XML
ScrollView catalogo = (ScrollView) findViewById(R.id.scroll_view);
// Atribuir uma função para quando o botão for clicado
goToPaos.setOnClickListener(new View.OnClickListener() {
            @Override	// sobrescrever o métoro onClick
            public void onClick(View v) {
                // Parâmetros: (ScrollView, View destino)
                scrollToView(catalogo,findViewById(R.id.textView3));
            }
        });
```

### Métodos da classe

```java
/**
     * Método para Scrolar a uma View específica.
     *
     * @param scrollViewParent Parent ScrollView
     * @param view View a qual queremos ir.
     */
private void scrollToView(final ScrollView scrollViewParent, final View view) {
    // Get deepChild Offset
    Point childOffset = new Point();
    getDeepChildOffset(scrollViewParent, view.getParent(), view, childOffset);
    // Scroll to child.
    scrollViewParent.smoothScrollTo(0, childOffset.y);
}

/**
     * Used to get deep child offset.
     * <p/>
     * 1. We need to scroll to child in scrollview, but the child may not the direct child to scrollview.
     * 2. So to get correct child position to scroll, we need to iterate through all of its parent views till the main parent.
     *
     * @param mainParent        Main Top parent.
     * @param parent            Parent.
     * @param child             Child.
     * @param accumulatedOffset Accumulated Offset.
     */
private void getDeepChildOffset(final ViewGroup mainParent, final ViewParent parent, final View child, final Point accumulatedOffset) {
    ViewGroup parentGroup = (ViewGroup) parent;
    accumulatedOffset.x += child.getLeft();
    accumulatedOffset.y += child.getTop();
    if (parentGroup.equals(mainParent)) {
        return;
    }
    getDeepChildOffset(mainParent, parentGroup.getParent(), parentGroup, accumulatedOffset);
}
```



## Receber dados JSON

## Enviar dados JSON

