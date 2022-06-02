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



## Atribuir uma função a um botão

```java
// No XML:
<?xml version="1.0" encoding="utf-8"?>
<Button xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/button_send"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/button_send"
    android:onClick="sendMessage" />  // <----- ATIVAR MÉTODO QUANDO CLICADO
```

```java
// Na classe que hospeda o botão:
public void sendMessage(View view) {
    // Insira aqui as ações que o botão irá executar
}
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



## Requisição HTTP GET com Volley

```java
// Habilitar permissão de acesso a internet no AndroidManifest.xml
<uses-permission android:name="android.permission.INTERNET" />
```

```java
// No build.gradle (Module:...) inserir o Volley nas dependências
dependencies {
        ...
        implementation 'com.android.volley:volley:1.2.1'
    }
```

```java
// Classe: destinada à realizar requisições
// Atribuir uma textview à uma variável
TextView textView = findViewById(R.id.text);

// Inicialização de uma lista de requisições
RequestQueue queue = Volley.newRequestQueue(this);

// String contendo o endereço http
String url = "http://www.google.com";

// Requisitar uma string como resposta da url consultada
// StringRequest -> Clase do volley, responsável por requisitar uma string
// Parâmetros de construção: método de requisição, string url, response.listener e errorlistener
StringRequest stringRequest = new StringRequest(
    Request.Method.GET,
    url,
    new Response.Listener<String>() {
        @Override
        public void onResponse(String response) { // Método que irá receber a resposta da url
            // Mostra os primeiros 500 caracteres da string requisitada
            textView.setText("Response is: "+ response.substring(0,500));
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            // Caso a requisição não funcione, mostra um erro
            textView.setText("That didn't work!");
        }
    });

// Envia a solicitação, adicionada a requisição à lista de requisições
queue.add(stringRequest);
}
```



## Enviar dados JSON

