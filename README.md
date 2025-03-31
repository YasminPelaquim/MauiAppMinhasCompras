# AppMinhasCompras - Documentação Completa

## Sumário

1. [Introdução](#introdução)
2. [Instalação e Configuração](#instalação-e-configuração)
3. [Arquitetura da Aplicação](#arquitetura-da-aplicação)
4. [Interface do Usuário](#interface-do-usuário)
5. [Análise do Código-Fonte](#análise-do-código-fonte)
   - [Estrutura do Projeto](#estrutura-do-projeto)
   - [Models](#models)
   - [ViewModels](#viewmodels)
   - [Views](#views)
   - [Services](#services)
   - [Helpers](#helpers)
6. [Fluxo de Funcionamento](#fluxo-de-funcionamento)
7. [Boas Práticas Implementadas](#boas-práticas-implementadas)
8. [Possíveis Melhorias](#possíveis-melhorias)
9. [Considerações Finais](#considerações-finais)
10. [Referências](#referências)

## Introdução

O **AppMinhasCompras** é uma aplicação mobile desenvolvida utilizando .NET MAUI (Multi-platform App UI), que permite aos usuários gerenciar suas listas de compras de forma prática e intuitiva. A aplicação foi desenvolvida com foco em usabilidade e performance, utilizando o padrão MVVM (Model-View-ViewModel).

O aplicativo oferece funcionalidades como:
- Criação e gerenciamento de múltiplas listas de compras
- Adição, edição e remoção de itens
- Marcação de itens como comprados
- Visualização do valor total da lista
- Organização por categorias
- Interface intuitiva e responsiva

## Instalação e Configuração

### Requisitos

Para executar e desenvolver o AppMinhasCompras, você precisará de:

- Visual Studio 2022 ou posterior com cargas de trabalho para desenvolvimento .NET MAUI
- .NET 7.0 SDK ou posterior
- Emuladores Android/iOS ou dispositivos físicos para testes

### Clonando o Repositório

```bash
git clone https://github.com/YasminPelaquim/MauiAppMinhasCompras.git
cd MauiAppMinhasCompras
```

### Executando a Aplicação

1. Abra a solução `MauiAppMinhasCompras.sln` no Visual Studio
2. Selecione a plataforma de destino (Android, iOS, Windows)
3. Pressione F5 ou clique em "Iniciar Depuração"

## Arquitetura da Aplicação

O AppMinhasCompras segue a arquitetura MVVM (Model-View-ViewModel), que proporciona uma separação clara entre a interface do usuário e a lógica de negócios. A estrutura está organizada da seguinte forma:

```
AppMinhasCompras/
├── Models/             # Classes de dados e entidades
├── ViewModels/         # Lógica de apresentação e comandos
├── Views/              # Interfaces XAML e código por trás
├── Services/           # Serviços para acesso a dados e lógicas específicas
├── Helpers/            # Classes auxiliares e utilitárias
└── Resources/          # Recursos de estilo, imagens e fontes
```

### Padrão MVVM

- **Model**: Representa as entidades de dados, como `Produto` e `ListaCompra`
- **View**: Interfaces de usuário em XAML que exibem os dados
- **ViewModel**: Ponte entre Model e View, gerenciando a lógica de apresentação

## Interface do Usuário

### Tela Principal

![Image](https://github.com/user-attachments/assets/2d09482d-df86-4789-85f4-6be6ddb4fc92)






A tela principal exibe todas as listas de compras criadas pelo usuário, com opções para adicionar novas listas, editar ou excluir existentes.

### Soma dos Itens da Lista

![Image](https://github.com/user-attachments/assets/7ac2e751-1208-4493-b118-69a4abdba08b)
Mostra o valor total da lista.

### Adição/Edição de Item

![Image](https://github.com/user-attachments/assets/f8d73286-0a04-4445-bb2e-0cf52da880f9)
![Image](https://github.com/user-attachments/assets/ffdda88f-9b2b-4803-9510-bf2c6f77c6ab)
Interface para adicionar ou editar itens, permitindo inserir nome, quantidade, preço e selecionar categoria.

### Funcionamento

https://github.com/user-attachments/assets/3eb8e350-37d6-4b79-8bcb-a428cf2fd509

*GIF mostrando o fluxo básico de uso do aplicativo, desde a criação de uma lista até a marcação de itens como comprados.*

## Análise do Código-Fonte

### Estrutura do Projeto

O projeto está estruturado seguindo as convenções do .NET MAUI com o padrão MVVM:

```
MauiAppMinhasCompras/
├── MauiAppMinhasCompras.csproj  # Arquivo de projeto
├── MauiProgram.cs               # Configuração e injeção de dependências
├── App.xaml                     # Recursos globais da aplicação
├── AppShell.xaml                # Estrutura de navegação Shell
├── Models/                      # Classes de modelo
│   ├── Categoria.cs
│   ├── ListaCompra.cs
│   └── Produto.cs
├── ViewModels/                  # ViewModels
│   ├── BaseViewModel.cs
│   ├── ListaComprasViewModel.cs
│   ├── DetalheListaViewModel.cs
│   └── ProdutoViewModel.cs
├── Views/                       # Páginas e controles
│   ├── ListaComprasPage.xaml
│   ├── DetalheListaPage.xaml
│   └── ProdutoPage.xaml
├── Services/                    # Serviços
│   ├── IDataService.cs
│   └── DataService.cs
├── Helpers/                     # Classes auxiliares
│   ├── Converters.cs
│   └── Utilities.cs
└── Resources/                   # Recursos
    ├── Images/
    ├── Fonts/
    └── Styles/
```

### Models

#### Produto.cs

```csharp
namespace MauiAppMinhasCompras.Models
{
    public class Produto
    {
        public int Id { get; set; }
        public string Nome { get; set; }
        public double Quantidade { get; set; }
        public double Preco { get; set; }
        public int CategoriaId { get; set; }
        public bool Comprado { get; set; }
        public int ListaCompraId { get; set; }

        public double ValorTotal => Quantidade * Preco;
    }
}
```

A classe `Produto` representa um item da lista de compras com propriedades para identificação, nome, quantidade, preço, categoria, status de compra e lista relacionada. Ela também inclui uma propriedade calculada `ValorTotal` que retorna o preço total do item baseado na quantidade.

#### ListaCompra.cs

```csharp
namespace MauiAppMinhasCompras.Models
{
    public class ListaCompra
    {
        public int Id { get; set; }
        public string Nome { get; set; }
        public DateTime DataCriacao { get; set; }
        public List<Produto> Produtos { get; set; } = new List<Produto>();

        public double ValorTotal => Produtos.Sum(p => p.ValorTotal);
        public double ValorTotalComprado => Produtos.Where(p => p.Comprado).Sum(p => p.ValorTotal);
        public int TotalItens => Produtos.Count;
        public int TotalItensComprados => Produtos.Count(p => p.Comprado);
    }
}
```

A classe `ListaCompra` representa uma lista de compras com propriedades para identificação, nome, data de criação e uma coleção de produtos. Ela inclui propriedades calculadas para o valor total da lista, valor total dos itens comprados, contagem de itens e contagem de itens comprados.

#### Categoria.cs

```csharp
namespace MauiAppMinhasCompras.Models
{
    public class Categoria
    {
        public int Id { get; set; }
        public string Nome { get; set; }
        public string Icone { get; set; }
    }
}
```

A classe `Categoria` representa uma categoria de produtos com identificação, nome e ícone representativo.

### ViewModels

#### BaseViewModel.cs

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace MauiAppMinhasCompras.ViewModels
{
    public class BaseViewModel : INotifyPropertyChanged
    {
        private bool _isBusy;
        public bool IsBusy
        {
            get => _isBusy;
            set => SetProperty(ref _isBusy, value);
        }

        private string _title;
        public string Title
        {
            get => _title;
            set => SetProperty(ref _title, value);
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
        {
            if (EqualityComparer<T>.Default.Equals(storage, value))
                return false;

            storage = value;
            OnPropertyChanged(propertyName);
            return true;
        }

        protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

`BaseViewModel` é a classe base para todos os ViewModels, implementando `INotifyPropertyChanged` para notificar a interface quando propriedades são alteradas. Ela inclui propriedades comuns como `IsBusy` e `Title`, além do método `SetProperty` para facilitar a implementação de propriedades que notificam suas mudanças.

#### ListaComprasViewModel.cs

```csharp
using MauiAppMinhasCompras.Models;
using MauiAppMinhasCompras.Services;
using MauiAppMinhasCompras.Views;
using System.Collections.ObjectModel;
using System.Windows.Input;

namespace MauiAppMinhasCompras.ViewModels
{
    public class ListaComprasViewModel : BaseViewModel
    {
        private readonly IDataService _dataService;
        public ObservableCollection<ListaCompra> ListasCompras { get; } = new ObservableCollection<ListaCompra>();
        
        public ICommand NovaListaCommand { get; }
        public ICommand AbrirListaCommand { get; }
        public ICommand ExcluirListaCommand { get; }
        
        private string _novaListaNome;
        public string NovaListaNome
        {
            get => _novaListaNome;
            set => SetProperty(ref _novaListaNome, value);
        }
        
        public ListaComprasViewModel(IDataService dataService)
        {
            _dataService = dataService;
            Title = "Minhas Listas de Compras";
            
            NovaListaCommand = new Command(AdicionarNovaLista);
            AbrirListaCommand = new Command<ListaCompra>(AbrirLista);
            ExcluirListaCommand = new Command<ListaCompra>(ExcluirLista);
            
            CarregarListas();
        }
        
        private async void CarregarListas()
        {
            IsBusy = true;
            
            try
            {
                ListasCompras.Clear();
                var listas = await _dataService.ObterListasComprasAsync();
                foreach (var lista in listas)
                {
                    ListasCompras.Add(lista);
                }
            }
            catch (Exception ex)
            {
                // Tratar erro
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível carregar as listas: {ex.Message}", "OK");
            }
            finally
            {
                IsBusy = false;
            }
        }
        
        private async void AdicionarNovaLista()
        {
            if (string.IsNullOrWhiteSpace(NovaListaNome))
            {
                await Shell.Current.DisplayAlert("Erro", "Nome da lista não pode estar vazio", "OK");
                return;
            }
            
            IsBusy = true;
            
            try
            {
                var novaLista = new ListaCompra
                {
                    Nome = NovaListaNome,
                    DataCriacao = DateTime.Now
                };
                
                await _dataService.SalvarListaCompraAsync(novaLista);
                ListasCompras.Add(novaLista);
                NovaListaNome = string.Empty;
            }
            catch (Exception ex)
            {
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível adicionar a lista: {ex.Message}", "OK");
            }
            finally
            {
                IsBusy = false;
            }
        }
        
        private async void AbrirLista(ListaCompra lista)
        {
            if (lista == null)
                return;
                
            await Shell.Current.GoToAsync($"{nameof(DetalheListaPage)}?listaId={lista.Id}");
        }
        
        private async void ExcluirLista(ListaCompra lista)
        {
            if (lista == null)
                return;
                
            bool confirmar = await Shell.Current.DisplayAlert("Confirmar", 
                $"Deseja realmente excluir a lista '{lista.Nome}'?", "Sim", "Não");
                
            if (!confirmar)
                return;
                
            IsBusy = true;
            
            try
            {
                await _dataService.ExcluirListaCompraAsync(lista.Id);
                ListasCompras.Remove(lista);
            }
            catch (Exception ex)
            {
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível excluir a lista: {ex.Message}", "OK");
            }
            finally
            {
                IsBusy = false;
            }
        }
    }
}
```

`ListaComprasViewModel` gerencia a exibição e interação com a lista de compras na tela principal. Implementa comandos para adicionar, abrir e excluir listas, além de gerenciar o carregamento de dados através do `DataService`.

#### DetalheListaViewModel.cs

```csharp
using MauiAppMinhasCompras.Models;
using MauiAppMinhasCompras.Services;
using MauiAppMinhasCompras.Views;
using System.Collections.ObjectModel;
using System.Windows.Input;

namespace MauiAppMinhasCompras.ViewModels
{
    [QueryProperty(nameof(ListaId), "listaId")]
    public class DetalheListaViewModel : BaseViewModel
    {
        private readonly IDataService _dataService;
        public ObservableCollection<Produto> Produtos { get; } = new ObservableCollection<Produto>();
        
        private int _listaId;
        public int ListaId
        {
            get => _listaId;
            set
            {
                SetProperty(ref _listaId, value);
                CarregarListaDetalhes();
            }
        }
        
        private ListaCompra _listaCompra;
        public ListaCompra ListaCompra
        {
            get => _listaCompra;
            set => SetProperty(ref _listaCompra, value);
        }
        
        public ICommand NovoProdutoCommand { get; }
        public ICommand ToggleCompradoCommand { get; }
        public ICommand EditarProdutoCommand { get; }
        public ICommand ExcluirProdutoCommand { get; }
        
        public DetalheListaViewModel(IDataService dataService)
        {
            _dataService = dataService;
            
            NovoProdutoCommand = new Command(AdicionarProduto);
            ToggleCompradoCommand = new Command<Produto>(ToggleComprado);
            EditarProdutoCommand = new Command<Produto>(EditarProduto);
            ExcluirProdutoCommand = new Command<Produto>(ExcluirProduto);
        }
        
        private async void CarregarListaDetalhes()
        {
            IsBusy = true;
            
            try
            {
                ListaCompra = await _dataService.ObterListaCompraAsync(ListaId);
                
                if (ListaCompra != null)
                {
                    Title = ListaCompra.Nome;
                    Produtos.Clear();
                    
                    foreach (var produto in ListaCompra.Produtos)
                    {
                        Produtos.Add(produto);
                    }
                }
            }
            catch (Exception ex)
            {
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível carregar os detalhes da lista: {ex.Message}", "OK");
            }
            finally
            {
                IsBusy = false;
            }
        }
        
        private async void AdicionarProduto()
        {
            await Shell.Current.GoToAsync($"{nameof(ProdutoPage)}?listaId={ListaId}");
        }
        
        private async void ToggleComprado(Produto produto)
        {
            if (produto == null)
                return;
                
            produto.Comprado = !produto.Comprado;
            
            try
            {
                await _dataService.SalvarProdutoAsync(produto);
                OnPropertyChanged(nameof(ListaCompra.ValorTotal));
                OnPropertyChanged(nameof(ListaCompra.ValorTotalComprado));
            }
            catch (Exception ex)
            {
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível atualizar o produto: {ex.Message}", "OK");
                produto.Comprado = !produto.Comprado; // Reverte em caso de erro
            }
        }
        
        private async void EditarProduto(Produto produto)
        {
            if (produto == null)
                return;
                
            await Shell.Current.GoToAsync($"{nameof(ProdutoPage)}?listaId={ListaId}&produtoId={produto.Id}");
        }
        
        private async void ExcluirProduto(Produto produto)
        {
            if (produto == null)
                return;
                
            bool confirmar = await Shell.Current.DisplayAlert("Confirmar", 
                $"Deseja realmente excluir o produto '{produto.Nome}'?", "Sim", "Não");
                
            if (!confirmar)
                return;
                
            IsBusy = true;
            
            try
            {
                await _dataService.ExcluirProdutoAsync(produto.Id);
                Produtos.Remove(produto);
                ListaCompra.Produtos.Remove(produto);
                OnPropertyChanged(nameof(ListaCompra.ValorTotal));
                OnPropertyChanged(nameof(ListaCompra.ValorTotalComprado));
            }
            catch (Exception ex)
            {
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível excluir o produto: {ex.Message}", "OK");
            }
            finally
            {
                IsBusy = false;
            }
        }
    }
}
```

`DetalheListaViewModel` gerencia a exibição e interação com os detalhes de uma lista específica, incluindo seus produtos. Implementa comandos para adicionar, marcar como comprado, editar e excluir produtos.

#### ProdutoViewModel.cs

```csharp
using MauiAppMinhasCompras.Models;
using MauiAppMinhasCompras.Services;
using System.Collections.ObjectModel;
using System.Windows.Input;

namespace MauiAppMinhasCompras.ViewModels
{
    [QueryProperty(nameof(ListaId), "listaId")]
    [QueryProperty(nameof(ProdutoId), "produtoId")]
    public class ProdutoViewModel : BaseViewModel
    {
        private readonly IDataService _dataService;
        public ObservableCollection<Categoria> Categorias { get; } = new ObservableCollection<Categoria>();
        
        private int _listaId;
        public int ListaId
        {
            get => _listaId;
            set => SetProperty(ref _listaId, value);
        }
        
        private int _produtoId;
        public int ProdutoId
        {
            get => _produtoId;
            set
            {
                SetProperty(ref _produtoId, value);
                if (value > 0)
                {
                    CarregarProduto();
                }
                else
                {
                    Produto = new Produto { ListaCompraId = ListaId };
                    Title = "Novo Produto";
                }
            }
        }
        
        private Produto _produto;
        public Produto Produto
        {
            get => _produto;
            set => SetProperty(ref _produto, value);
        }
        
        private int _categoriaId;
        public int CategoriaId
        {
            get => _categoriaId;
            set
            {
                SetProperty(ref _categoriaId, value);
                if (Produto != null)
                {
                    Produto.CategoriaId = value;
                }
            }
        }
        
        public ICommand SalvarCommand { get; }
        public ICommand CancelarCommand { get; }
        
        public ProdutoViewModel(IDataService dataService)
        {
            _dataService = dataService;
            
            Produto = new Produto();
            
            SalvarCommand = new Command(SalvarProduto);
            CancelarCommand = new Command(Cancelar);
            
            CarregarCategorias();
        }
        
        private async void CarregarCategorias()
        {
            try
            {
                Categorias.Clear();
                var categorias = await _dataService.ObterCategoriasAsync();
                foreach (var categoria in categorias)
                {
                    Categorias.Add(categoria);
                }
            }
            catch (Exception ex)
            {
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível carregar as categorias: {ex.Message}", "OK");
            }
        }
        
        private async void CarregarProduto()
        {
            IsBusy = true;
            
            try
            {
                Produto = await _dataService.ObterProdutoAsync(ProdutoId);
                
                if (Produto != null)
                {
                    Title = "Editar Produto";
                    CategoriaId = Produto.CategoriaId;
                }
            }
            catch (Exception ex)
            {
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível carregar o produto: {ex.Message}", "OK");
            }
            finally
            {
                IsBusy = false;
            }
        }
        
        private async void SalvarProduto()
        {
            if (string.IsNullOrWhiteSpace(Produto.Nome))
            {
                await Shell.Current.DisplayAlert("Erro", "Nome do produto não pode estar vazio", "OK");
                return;
            }
            
            if (Produto.Quantidade <= 0)
            {
                await Shell.Current.DisplayAlert("Erro", "Quantidade deve ser maior que zero", "OK");
                return;
            }
            
            IsBusy = true;
            
            try
            {
                await _dataService.SalvarProdutoAsync(Produto);
                await Shell.Current.GoToAsync("..");
            }
            catch (Exception ex)
            {
                await Shell.Current.DisplayAlert("Erro", $"Não foi possível salvar o produto: {ex.Message}", "OK");
            }
            finally
            {
                IsBusy = false;
            }
        }
        
        private async void Cancelar()
        {
            await Shell.Current.GoToAsync("..");
        }
    }
}
```

`ProdutoViewModel` gerencia a adição e edição de produtos. Ele recebe parâmetros através de QueryProperties para determinar se está criando um novo produto ou editando um existente, e carrega as categorias disponíveis para seleção.

### Views

#### ListaComprasPage.xaml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:vm="clr-namespace:MauiAppMinhasCompras.ViewModels"
             x:Class="MauiAppMinhasCompras.Views.ListaComprasPage"
             Title="{Binding Title}">

    <Grid RowDefinitions="Auto, *">
        <!-- Formulário para adicionar nova lista -->
        <Frame Margin="10" Padding="10" BorderColor="LightGray">
            <Grid ColumnDefinitions="*, Auto">
                <Entry Placeholder="Nome da nova lista" 
                       Text="{Binding NovaListaNome}" 
                       ReturnCommand="{Binding NovaListaCommand}" />
                <Button Grid.Column="1" 
                        Text="Adicionar" 
                        Command="{Binding NovaListaCommand}" 
                        Margin="5,0,0,0" />
            </Grid>
        </Frame>

        <!-- Lista de compras -->
        <RefreshView Grid.Row="1" 
                     IsRefreshing="{Binding IsBusy}" 
                     Command="{Binding RefreshCommand}">
            <CollectionView ItemsSource="{Binding ListasCompras}"
                            EmptyView="Você ainda não possui listas de compras. Adicione sua primeira lista!">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <SwipeView>
                            <SwipeView.RightItems>
                                <SwipeItems>
                                    <SwipeItem Text="Excluir" 
                                               BackgroundColor="Red" 
                                               Command="{Binding Source={RelativeSource AncestorType={x:Type vm:ListaComprasViewModel}}, Path=ExcluirListaCommand}" 
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.RightItems>
                            
                            <Frame Margin="10,5" Padding="10" BorderColor="LightGray">
                                <Frame.GestureRecognizers>
                                    <TapGestureRecognizer 
                                        Command="{Binding Source={RelativeSource AncestorType={x:Type vm:ListaComprasViewModel}}, Path=AbrirListaCommand}" 
                                        CommandParameter="{Binding}" />
                                </Frame.GestureRecognizers>
                                
                                <Grid RowDefinitions="Auto, Auto" ColumnDefinitions="*, Auto">
                                    <Label Text="{Binding Nome}" 
                                           FontSize="18" 
                                           FontAttributes="Bold" />
                                    <Label Grid.Row="1" 
                                           Text="{Binding DataCriacao, StringFormat='Criada em: {0:dd/MM/yyyy}'}" 
                                           FontSize="14" 
                                           TextColor="Gray" />
                                    <VerticalStackLayout Grid.Column="1" Grid.RowSpan="2" VerticalOptions="Center">
                                        <Label Text="{Binding ValorTotal, StringFormat='R$ {0:F2}'}" 
                                               FontSize="16" 
                                               HorizontalOptions="End" />
                                        <Label Text="{Binding TotalItens, StringFormat='{0} itens'}" 
                                               FontSize="14" 
                                               TextColor="Gray" 
                                               HorizontalOptions="End" />
                                    </VerticalStackLayout>
                                </Grid>
                            </Frame>
                        </SwipeView>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </RefreshView>
    </Grid>
</ContentPage>
```

Essa página exibe a lista de compras do usuário e permite adicionar novas listas. Utiliza um CollectionView para exibir as listas e SwipeView para habilitar a exclusão através de gesto de swipe.

#### DetalheListaPage.xaml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:vm="clr-namespace:MauiAppMinhasCompras.ViewModels"
             x:Class="MauiAppMinhasCompras.Views.DetalheListaPage"
             Title="{Binding Title}">

    <Grid RowDefinitions="Auto, *, Auto">
        <!-- Cabeçalho com informações da lista -->
        <Frame Margin="10,10,10,0" Padding="10" BorderColor="LightGray">
            <Grid ColumnDefinitions="*, *">
                <VerticalStackLayout>
                    <Label Text="Valor Total" FontSize="14" TextColor="Gray" />
                    <Label Text="{Binding ListaCompra.ValorTotal, StringFormat='R$ {0:F2}'}" 
                           FontSize="20" 
                           FontAttributes="Bold" />
                    <Label Text="{Binding ListaCompra.TotalItens, StringFormat='{0} itens'}" 
                           FontSize="14" 
                           TextColor="Gray" />
                </VerticalStackLayout>
                
                <VerticalStackLayout Grid.Column="1" HorizontalOptions="End">
                    <Label Text="Comprado" FontSize="14" TextColor="Gray" />
                    <Label Text="{Binding ListaCompra.ValorTotalComprado, StringFormat='R$ {0:F2}'}" 
                           FontSize="20" 
                           FontAttributes="Bold" />
                    <Label Text="{Binding ListaCompra.TotalItensComprados, StringFormat='{0} itens'}" 
                           FontSize="14" 
                           TextColor="Gray" />
                </VerticalStackLayout>
            </Grid>
        </Frame>

        <!-- Lista de produtos -->
        <RefreshView Grid.Row="1" 
                     IsRefreshing="{Binding IsBusy}" 
                     Command="{Binding RefreshCommand}">
            <CollectionView ItemsSource="{Binding Produtos}"
                            EmptyView="Nenhum produto adicionado a esta lista.">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <SwipeView>
                            <SwipeView.LeftItems>
                                <SwipeItems>
                                    <SwipeItem Text="Editar" 
                                               BackgroundColor="Blue" 
                                               Command="{Binding Source={RelativeSource AncestorType={x:Type vm:DetalheListaViewModel}}, Path=EditarProdutoCommand}" 
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.LeftItems>
                            <SwipeView.RightItems>
                                <SwipeItems>
                                    <SwipeItem Text="Excluir" 
                                               BackgroundColor="Red"
