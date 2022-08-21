##########################################
###### CRIAÇÃO DE TABELAS EM DJANGO ######
##########################################

- Para criação de tabelas se usa classes, no app criado iremos mexer na parte de models.py
Vejamos um exemplo prático abaixo:


1 from django.db import models
2 from django.utils import timezone
3
4
5 class Categoria(models.Model):
6 nome = models.CharField(max_length=255)
7
8 def __str__(self):
9 return self.nome
10
11
12 class Contato(models.Model):
13 nome = models.CharField(max_length=255)
14 sobrenome = models.CharField(max_length=255, blank=True)
15 telefone = models.CharField(max_length=255)
16 email = models.CharField(max_length=255, blank=True)
17 data_criacao = models.DateTimeField(default=timezone.now)
18 descricao = models.TextField(blank=True)
19 categoria = models.ForeignKey(Categoria, on_delete=models.DO_NOTHING)
20
21 def __str__(self):
22 return self.nome


Após a criação das classes(tabelas), vamos agora fazer as migrações dessas tabelas criadas.
Comando para migrações:
-> python manage.py makemigrations
-> python manage.py migrate

Pronto suas classes foram inseridas no banco dados de fato!
Agora para visualizar vamos ate a área administrativas do Django, uma das parte mais poderas eu diria do Django.


Rodamos o servidor, entramos no admin, mais para entrar precisamos criar também o login de entrada no painel.
Comando para criação de login são:
-> python manage.py createsuperuser
-> username: testando
-> email: testando@gmail.com
-> password: testando123
-> password(again): testando123
-> yes

Criado vamos ter agora que mexer na parte de admin.py do nosso app
iremos registrar as classes criadas para podermos visualizar no painel administrativo
vejamos o exemplo a seguir:

1 from django.contrib import admin
2 from .models import Categoria, Contato
3
4 class ContatoAdmin(admin.ModelAdmin):
5 list_display = ('id', 'nome', 'sobrenome', 'telefone', 'email', 'data_criacao', 'categoria')
6 list_display_liks = ('id', 'nome', 'sobrenome', 'email')
7 list_filter = ('nome', 'sobrenome', 'data_criacao')
8 list_per_page = 10
9 search_fields = ('nome', 'sobrenome', 'telefone', 'email')

site.admin.register(Categoria)
site.admin.register(Contato, ContatoAdmin)


Toda configuração principal da parte de administração ja esta feita.
Vamos agora mostrar como exibir esses dados na tela principal do nosso site
Pra isso vamos mexer no views.py do nosso app e importar nosso classe que queremos mostrar
Vejamos exemplo de como ficaria:

1 from django.shortcuts import render
2 from .models import Categoria

3 def index(request):
4    return render(request, 'contatos/index.html')

Por padrão no Django podemos inserir um parâmetro a mais para mostrar campos do nosso banco de dados criados.
Vamos apenas modificar nosso função criada onde se encontra o template de nosso site e depois mexer no 
template com as programação do proprio Django.
Vamos ver o código para melhor entendimento:


def index(request):
    contatos = Contato.objects.all()
    return render(request, 'contatos/index.html', {
        'contatos': contatos        
    })


