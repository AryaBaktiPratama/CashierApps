# The Cashier Apps
Aplikasi ini merupakan aplikasi kasir sederhana yang berfungsi untuk melakukan perhitungan transaksi barang/jasa dengan cara menginputkan nama, type, harga dan jumlah barang/jasanya.
## Scope & Functionalities
- User dapat menginputkan Nama Barang/Jasa
- User dapat memilih type yang sesuai yaitu Barang/Jasa
- User dapat menginputkan jumlah dan harga Barang/Jasa
- Pada bagian jumlah dan harga User harus menginputkan dengan angka saja (tidak boleh karakter/huruf)
- Total harga dari semua Barang/Jasa yang telah diinputkan terletak pada Label paling bawah

## How Does it works?
- Diawali dari Method `MainWindow` pada class `MainWindow.xaml.cs` kita mendeklarasikan :
```csharp
private Calculator calculator;
    public MainWindow()
    {
        InitializeComponent();
        calculator = new Calculator();
        listBox1.ItemsSource = calculator.GetListItem();
    }
```
- Inisiasi List dan Perhitungan Barang/Jasa terdapat pada class `Item.cs` :
```csharp
public string GetTitle()
{
    return title;
}
public int GetQuantity()
{
    return quantity;
}
public double GetPrice()
{
    return price;
}
public double GetSubTotal()
{
    subtotal = price * quantity;
    return subtotal;
}
public string GetType()
{
    return type;
}
```
- Logika Perhitungan Total Seluruh Barang/Jasa yang telah diinputkan berada pada Class `Calculator.cs` :
```csharp
public void AddItem(Item item)
    {
        this.listItem.Add(item);
        this.total += item.GetSubTotal();
    }
```
- Cara menampilkan apa yang telah diinputkan ke dalam ListBox saat menekan Tambahkan (Button) :
```csharp
private void AddButton_Click(object sender, RoutedEventArgs e)
{
    string title = itemNameBox.Text;
    int quantity = int.Parse(quantityBox.Text);
    string type = typeBox.Text;
    double price = double.Parse(priceBox.Text);

    Item item = new Item(new Random().Next(), title, quantity, price, price, type);
    calculator.AddItem(item);
    double total = calculator.GetTotal();

    totalLabel.Content = String.Format("Rp {0}", total);
    listBox1.Items.Refresh();
}
```
- Bagian yang menerapkan prinsip Single Responsibility :
```csharp
    public partial class MainWindow : Window
    {
        private Calculator calculator;
        public MainWindow()
        {
            InitializeComponent();
            calculator = new Calculator();
            listBox.ItemsSource = calculator.getListItem();
        }
```
- Pada bagian diatas Class `Calculator.cs` telah di deklarasikan dan di inisialisasi
- Class `Calculator.cs` :
```csharp
    class Calculator
    {
        private List<Item> listItem;
        private double total = 0;

        public Calculator()
        {
            this.listItem = new List<Item>();
        }

        public void addItem(Item item)
        {
            this.listItem.Add(item);
            this.total += item.getSubtotal();
        }

        public double getTotal()
        {
            return total;
        }

        public List<Item> getListItem()
        {
            return listItem;
        }
    }
}
```