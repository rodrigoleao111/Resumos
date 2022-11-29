[TOC]

# Sintaxes e tutoriais - Java Mobile

## Toast

```java
Toast.makeText(MainActivity.this, "Mensagem aqui", Toast.LENGTH_LONG).show();
// Parâmetros: contexto, mensagem, duração
```



## View como variável

```java
// nomeDaView = encontrarViewPeloID;
EditText edtNome = findViewById(R.id.editTextPersonName);
// (TipoVariável) nomeVariável = nomeDaView.(funções para pegar e converter dados)
String nome = edtNome.getText().toString();
```



## Atribuir uma função a um botão

### No XML:

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

### No código:

```java
private Buttom bt_confirma;

protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
 		
    bt_confirma.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // Faz alguma coisa aqui...
            }
        });
}
```



## Navegar entre Activities (e enviar dados via Bundle)

### Na Activity origem

```java
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



## Acessar localização do usuário via GPS

### Habilitar a API de localização como dependência no build.gradle(:app)

```java
dependencies {
...
implementation 'com.google.android.gms:play-services-location:19.0.1'
}
```

### Adicionar permissões no manifest

```java
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.INTERNET"/>
```



## Coletando imagens

### 1.Permissões no manifest

```java
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.CAMERA" />
```

### 2. Decidindo local e checando permissões

```java
private ImageView addFoto;	// Botão

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Adicionar imagem
    addFoto.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            showImagePicDialog();
        }
    });
}

public void showImagePicDialog() {
    cameraPermission = new String[]{Manifest.permission.CAMERA, Manifest.permission.WRITE_EXTERNAL_STORAGE};
    storagePermission = new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE};
    String[] options = {"Câmera", "Galeria"};
    AlertDialog.Builder builder = new AlertDialog.Builder(FormOSManutencaoCorretiva.this);
    builder.setTitle("Selecione a fonte da imagem:");
    builder.setItems(options, new DialogInterface.OnClickListener() {
        @RequiresApi(api = Build.VERSION_CODES.M)
        @Override
        public void onClick(DialogInterface dialog, int which) {
            if (which == 0) {
                if (!checkCameraPermission()) {
                    requestCameraPermission();
                } else {
                    Intent camera = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
                    cameraResultLauncher.launch(camera);
                }
            } else if (which == 1) {
                if (!checkStoragePermission()) {
                    requestStoragePermission();
                } else {
                    mGetContent.launch("image/*");
                }
            }
        }
    });
    builder.create().show();
}

// Checagem de permissão: armazenamento externo
public Boolean checkStoragePermission() {
    boolean result = ContextCompat.checkSelfPermission(getApplicationContext(), Manifest.permission.WRITE_EXTERNAL_STORAGE) == (PackageManager.PERMISSION_GRANTED);
    return result;
}

// Requisição de permissão: galeria
@RequiresApi(api = Build.VERSION_CODES.M)
private void requestStoragePermission() {
    requestPermissions(storagePermission, STORAGE_REQUEST);
}

// Checagem de permissão: camera
public Boolean checkCameraPermission() {
    boolean result = ContextCompat.checkSelfPermission(getApplicationContext(), Manifest.permission.CAMERA) == (PackageManager.PERMISSION_GRANTED);
    boolean result1 = ContextCompat.checkSelfPermission(getApplicationContext(), Manifest.permission.WRITE_EXTERNAL_STORAGE) == (PackageManager.PERMISSION_GRANTED);
    return result && result1;
}

// Requisição de permissão: camera
@RequiresApi(api = Build.VERSION_CODES.M)
private void requestCameraPermission() {
    requestPermissions(cameraPermission, CAMERA_REQUEST);
}
```

### 3. Da galeria

```java
// Coletar imagem da galeria
ActivityResultLauncher<String> mGetContent = registerForActivityResult(
    new ActivityResultContracts.GetContent(), new ActivityResultCallback<Uri>() {
        @Override
        public void onActivityResult(Uri result) {
            Intent sendToCropImageActivity = new Intent(getApplicationContext(), CropImage.class);
            sendToCropImageActivity.putExtra("uri", result);
            sendToCropImageActivity.putExtra("call", 1);
            sendToCropImageActivity.putExtra("source", 1);
            sendToCropImageActivity.putExtras(ColetarInformacoes());
            startActivity(sendToCropImageActivity);
        }
    });
```

### 4. Da câmera

```java
// Coletar imagem da camera
ActivityResultLauncher<Intent> cameraResultLauncher = registerForActivityResult(
    new ActivityResultContracts.StartActivityForResult(),
    new ActivityResultCallback<ActivityResult>() {
        @Override
        public void onActivityResult(ActivityResult result) {
            if (result.getResultCode() == Activity.RESULT_OK) {
                Intent data = result.getData();
                if (data != null) {		// Captura bem sucedida
                    // ASSIM ESTOU MODIFICANDO A TUMBNAIL (ESTICANDO A IMAGEM)
                    Bitmap cameraPic = (Bitmap)(data.getExtras().get("data"));
                    Uri tempUri = ImageHelper.getUriFromTumbnailBitmap(getApplicationContext(), cameraPic);

                    Intent sendToCropImageActivity = new Intent(getApplicationContext(), CropImage.class);
                    sendToCropImageActivity.putExtra("uri", tempUri);
                    sendToCropImageActivity.putExtra("call", 1);
                    sendToCropImageActivity.putExtra("source", 1);
                    sendToCropImageActivity.putExtras(ColetarInformacoes());
                    startActivity(sendToCropImageActivity);
                }
            }
        }
    });
```



## Tratamento de imagens

### Arredondar bordas de bitmap

```java
public static Bitmap getRoundedCornerBitmap(Bitmap bitmap, int pixels) {
    Bitmap output = Bitmap.createBitmap(bitmap.getWidth(), bitmap.getHeight(), Config.ARGB_8888);
    Canvas canvas = new Canvas(output);

    final int color = 0xff424242;
    final Paint paint = new Paint();
    final Rect rect = new Rect(0, 0, bitmap.getWidth(), bitmap.getHeight());
    final RectF rectF = new RectF(rect);
    final float roundPx = pixels;

    paint.setAntiAlias(true);
    canvas.drawARGB(0, 0, 0, 0);
    paint.setColor(color);
    canvas.drawRoundRect(rectF, roundPx, roundPx, paint);

    paint.setXfermode(new PorterDuffXfermode(Mode.SRC_IN));
    canvas.drawBitmap(bitmap, rect, rect, paint);

    return output;
}
```

### Recuperar Uri de tumbnail bitmap

```java
public static Uri getUriFromTumbnailBitmap(Context inContext, Bitmap inImage) {
    Bitmap OutImage = Bitmap.createScaledBitmap(inImage, inImage.getWidth()*10, inImage.getHeight()*10,true);
    String path = MediaStore.Images.Media.insertImage(inContext.getContentResolver(), OutImage, "Title", null);
    return Uri.parse(path);
}
```

### Recuperar path de uma Uri

```java
public static String getRealPathFromURI(Uri uri, Context inContext) {
    Cursor cursor = inContext.getContentResolver().query(uri, null, null, null, null);
    cursor.moveToFirst();
    int idx = cursor.getColumnIndex(MediaStore.Images.ImageColumns.DATA);
    return cursor.getString(idx);
}
```

### Bitmap >>> Byte Array

```java
public static byte[] getByteArrayFromBitmap(Bitmap bitmap){
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    bitmap.compress(Bitmap.CompressFormat.PNG, 100, bos);
    bitmap.recycle();
    return bos.toByteArray();
}
```

### Recuperar bitmap de diferentes tipos

```java
bitmap = BitmapFactory.decodeByteArray(bitmapdata, 0, bitmapdata.length);	// Byte Array
bitmap = MediaStore.Images.Media.getBitmap(getApplicationContext().getContentResolver(), uriBitmap);	// uri
```



## Salvar arquivo na memória

### PDF (Criar e salvar)

```java
public void GerarPDF(Bundle coletarInformacoes, Bitmap bitmap) throws IOException {

    // Chaves e informações
    String[] infoColaborador = {"Nome", "RG", "CPF", "Setor", "Cargo", "Telefone", "E-mail"};
    String[] infoEquipamento = {"Locação", "Equipamento", "Modelo", "ID"};
    String[] infoManutencao = {"Diagnóstico", "Solução", "Peças trocadas", "Observações"};
    String[] chavesColaborador = {"nome", "rg", "cpf", "setor", "cargo", "telefone", "email"};
    String[] chavesEquipamento = {"locacao", "equipamento", "modelo", "equipID"};
    String[] chavesManutencao = {"diagnostico", "solucao", "troca", "obs"};



    // Inicialização do pdf
    PdfDocument pdfRelatorio = new PdfDocument();
    Paint myPaint = new Paint();

    // Informações da página ----------------------------------------------------
    int pageWidth = 2480, pageHeight = 3508;  // Tamanho A4
    int y = 200, marginLeft = 115, center = pageWidth/2, marginRight = pageWidth-115,
    verticalSpacing = 50, imageSize = 900;
    float titleSize = 90.0f, subTitleSize = 70.0f, textSize = 40.0f, topcSize = 30.0f;
    PdfDocument.PageInfo infoRelatorio = new PdfDocument.PageInfo.Builder(pageWidth, pageHeight, 1).create();
    PdfDocument.Page pagRelatorio = pdfRelatorio.startPage(infoRelatorio);
    Canvas canvas = pagRelatorio.getCanvas();

    // Escrevendo na página ----------------------------------------------------
    // Título
    myPaint.setTextAlign(Paint.Align.CENTER);
    myPaint.setTextSize(titleSize);
    myPaint.setFakeBoldText(true);
    y += 90;
    canvas.drawText("Ordem de Serviço - " + coletarInformacoes.getString("formID"), center, y, myPaint);

    // Subtítulo
    myPaint.setTextSize(subTitleSize);
    myPaint.setFakeBoldText(false);
    myPaint.setColor(Color.rgb(112, 119, 119));
    y += 90;
    canvas.drawText("Manutenção corretiva", center, y, myPaint);

    // Data de preenchimento
    SimpleDateFormat formatter = new SimpleDateFormat("dd/MM/yyyy HH:mm:ss");
    Date date = new Date();
    y += 90;
    myPaint.setTextSize(subTitleSize - 10.0f);
    canvas.drawText(formatter.format(date), center, y, myPaint);

    // Informações do usuário
    myPaint.setTextAlign(Paint.Align.LEFT);
    myPaint.setTextSize(topcSize);
    y += 3*verticalSpacing;
    canvas.drawText("Informações do colaborador", marginLeft, y, myPaint);

    myPaint.setTextAlign(Paint.Align.LEFT);
    myPaint.setTextSize(textSize);
    myPaint.setColor(Color.BLACK);

    y += verticalSpacing;
    for (int i = 0; i < infoColaborador.length; i++) {
        canvas.drawText(infoColaborador[i] + ":", marginLeft, y, myPaint);
        canvas.drawText(coletarInformacoes.getString(chavesColaborador[i]), marginLeft + 200, y, myPaint);
        y += verticalSpacing;
    }

    y += 3*verticalSpacing;

    // Informações do equipamento
    myPaint.setColor(Color.rgb(112, 119, 119));
    myPaint.setTextAlign(Paint.Align.LEFT);
    myPaint.setTextSize(topcSize);
    canvas.drawText("Equipamento | Ativo", marginLeft, y, myPaint);

    myPaint.setTextAlign(Paint.Align.LEFT);
    myPaint.setTextSize(textSize);
    myPaint.setColor(Color.BLACK);

    y += verticalSpacing;

    for (int i = 0; i < infoEquipamento.length; i++) {
        canvas.drawText(infoEquipamento[i] + ":", marginLeft, y, myPaint);
        canvas.drawText(coletarInformacoes.getString(chavesEquipamento[i]), marginLeft + 300, y, myPaint);
        y += verticalSpacing;
    }

    y += 3*verticalSpacing;

    // Informações de Manutenção
    myPaint.setColor(Color.rgb(112, 119, 119));
    myPaint.setTextAlign(Paint.Align.LEFT);
    myPaint.setTextSize(topcSize);
    canvas.drawText("Informações de Manutenção", marginLeft, y, myPaint);

    myPaint.setTextAlign(Paint.Align.LEFT);
    myPaint.setTextSize(textSize);
    myPaint.setColor(Color.BLACK);

    y += verticalSpacing;

    for (int i = 0; i < infoManutencao.length; i++) {
        canvas.drawText(infoManutencao[i] + ":", marginLeft, y, myPaint);
        canvas.drawText(coletarInformacoes.getString(chavesManutencao[i]), marginLeft + 300, y, myPaint);
        y += verticalSpacing;
    }

    y += 3*verticalSpacing;

    // Imagem
    if(bitmap != null){
        Rect rectImage = new Rect( marginLeft, y, marginLeft+imageSize, y+imageSize);
        canvas.drawBitmap(bitmap, null, rectImage, myPaint);
        y += 2*verticalSpacing;
    }

    // Logo In Forma
    Resources res = getResources();
    Bitmap informaLogo = BitmapFactory.decodeResource(res, R.drawable.informalogopreto);
    Rect rect = new Rect(
        marginLeft,
        pageHeight - informaLogo.getHeight() - marginLeft,
        informaLogo.getWidth() + marginLeft,
        pageHeight - marginLeft);
    canvas.drawBitmap(informaLogo, null, rect, myPaint);

    pdfRelatorio.finishPage(pagRelatorio);

    // Construção do arquivo
    String nomeArquivo = "OSManutencao_" + coletarInformacoes.getString("formID") + ".pdf";
    File file = new File(UserInfo.getUserCredentials().getString("osPath"), nomeArquivo);
    file.setWritable(true);

    try {
        FileOutputStream fos = new FileOutputStream(file);
        pdfRelatorio.writeTo(fos);
        fos.flush();
        fos.close();
        Toast.makeText(getApplicationContext(), "arquivo gerado", Toast.LENGTH_SHORT).show();
        CompartilharRelatorio(file);
    } catch (IOException e) {
        e.printStackTrace();
    }

    pdfRelatorio.close();
}
```

### Imagem (Bitmap)

```java
// Construção do nome do arquivo
java.util.Date date = new Date();
String nomeArquivo = "DocShare-Image-" + userID + "-" + date.getTime() + ".png";
File file = new File(UserInfo.getUserCredentials().getString("imagesPath"), nomeArquivo);

// Criação do arquivo
try {
    file.createNewFile();
} catch (IOException e) {
    e.printStackTrace();
}

// Converter bitmap para byte array
ByteArrayOutputStream bos = new ByteArrayOutputStream();
finalBitmap.compress(Bitmap.CompressFormat.PNG, 0, bos);
byte[] bitmapdata = bos.toByteArray();

// Escrevendo no arquivo
FileOutputStream fos = null;
try {
    fos = new FileOutputStream(file);
    fos.write(bitmapdata);
    fos.flush();
    fos.close();
    Toast.makeText(getApplicationContext(), "Imagem salva com sucesso", Toast.LENGTH_SHORT).show();
} catch (IOException e) {
    e.printStackTrace();
}
```



## Compartilhando arquivos

### 1. Criar aqruivo file_path.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <external-path
        name="all_paths"
        path="." />
</paths>

```

### 2. Adicionar no manifest  > application

```xml
<provider
          android:name="androidx.core.content.FileProvider"
          android:authorities="com.example.docshare.provider"
          android:exported="false"
          android:grantUriPermissions="true" >
    <meta-data
               android:name="android.support.FILE_PROVIDER_PATHS"
               android:resource="@xml/file_path" />
</provider>
```

### 3. Função compartilhar PDF

```java
public void CompartilharRelatorio(File file) {

    Uri pathUri = FileProvider.getUriForFile(
        getApplicationContext(),
        "com.example.docshare.provider",
        file);

    if(file.exists()){
        Intent intentShare = new Intent(Intent.ACTION_SEND);
        intentShare.setType("application/pdf");
        intentShare.putExtra(Intent.EXTRA_STREAM, Uri.parse(pathUri.toString()));
        startActivity(Intent.createChooser(intentShare, "Compartilhar Ordem de Serviço"));
    } else {
        Toast.makeText(getApplicationContext(), "Arquivo não encontrado", Toast.LENGTH_SHORT).show();
    }
}
```



## Fragments

```

```



## XLM - Atributos interessantes

```xml

```

