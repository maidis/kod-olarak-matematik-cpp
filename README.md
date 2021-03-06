# kod-olarak-matematik-cpp

**Not:** Asıl proje, [math-as-code](https://github.com/Jam3/math-as-code)'dur. Asıl projenin geliştiricilerinden mattdesl, "Belgeyi Python'da yeniden yazmak için zamanım veya isteğim yok ama giriş kısmında diğer dil çatallamalarına bağlantı vermekten mutlu olurum." şeklinde [bir açıklama](https://github.com/Jam3/math-as-code/issues/61#issuecomment-422880159) yaptığı için ana depoya değişiklik isteğinin mümkün olmayacağı şimdiki bu yapı tercih edilmiştir.

Bu, geliştiricilere C++ kod karşılıklarını göstererek matematiksel notasyonu anlamalarını kolaylaştırmayı amaçlayan bir referanstır.

Motivasyon: Akademik makaleler, kendi kendini eğiten oyun ve grafik programcıları için korkutucu olabilir :)

Bu kılavuz henüz tamamlanmadı. Hata görürseniz veya katkıda bulunmak isterseniz, lütfen bir kayıt açın veya bir değişiklik isteği gönderin.


# önsöz
Matematiksel semboller; yazara, bağlama ve çalışma alanına (lineer cebir, küme teorisi, vb.) bağlı olarak farklı şeyler ifade edebilir. Bu kılavuz, bir sembolün tüm kullanımlarını kapsamayabilir. Bazı durumlarda, gerçek dünyada referansları (blog yazıları, yayınlar, vb.) verilerek bir sembolün vahşi ortamda nasıl görünebileceğini gösterilmeye çalışılacaktır.

Daha eksiksiz bir liste için Wikipedia'da [Matematiksel Sembollerin Listesi](https://en.wikipedia.org/wiki/List_of_mathematical_symbols)'ne bakınız.

Basitlik için, buradaki kod örneklerinin çoğu kayan nokta değerleri üzerinde çalışır ve sayısal olarak sağlam değildir. Bunun neden bir sorun olabileceğine dair daha çok ayrıntı için Mikola Lysenko'nun [Sağlam Aritmetik Notlarına](https://github.com/mikolalysenko/robust-arithmetic-notes) bakınız.


# içindekiler


## değişken isim kuralları
Çalışmanın bağlamına ve alanına bağlı olarak çeşitli isimlendirme kuralları vardır ve bunlar her zaman tutarlı değildir. Bununla birlikte, literatürde, aşağıdaki gibi bir deseni takip eden değişken isimleri bulabilirsiniz:

- *s* - skaler için italik küçük harfler (ör. bir sayı)
- **x** - vektörler için kalın küçük harfler (ör. bir 2B nokta)
- **A** - matrisler için büyük harfli harfler (ör. bir 3B dönüşüm)
- *θ* - sabitler ve özel değişkenler için italik küçük harfli Yunan harfleri (ör. [kutup açısı *θ*, *teta*](https://en.wikipedia.org/wiki/Spherical_coordinate_system))

Bu ayrıca bu kılavuzun da biçimi olacaktır.


## eşitlik sembolleri

Eşittir işareti ='e benzeyen bir dizi sembol vardır. Aşağıda birkaç genel örnek görülebilir:

- `=` eşitlik için kullanılır (değerler aynıdır)
- `≠` eşitsizlik için kullanılır (değer aynı değildir)
- `≈` yaklaşık olarak eşittir ifadesi için kullanılır (`π ≈ 3.14159`)
- `:=` tanımlama içindir (A, B olarak tanımlanmıştır)

C++'taysa:

```cpp
#include <iostream>
#include <cmath>

bool yaklasikEsitMi(double a, double b, double epsilon) {
  return fabs(a - b) <= epsilon;
}

int main()
{
    // eşitlik
    if (2==3)
        std::cout << "İki, üçe eşittir.\n";
    // eşitsizlik
    if (2!=3)
        std::cout << "İki, üçe eşit değildir.\n";
    // yaklaşık eşitlik
    if (yaklasikEsitMi(M_PI, 3.14159265359, 1e-5))
        std::cout << M_PI << ", yaklaşık olarak 3.14159265359 değerine eşittir.\n";

    return 0;
}
```

Tanımlama için `:=`, `=:` ve `=` sembollerinin kullanıldığını görebilirsiniz.<sup>[1]</sup>

Örneğin aşağıdaki tanımlama, x'i 2kj'nin farklı bir ismi olarak tanımlar:

![x := 2kj](http://latex.codecogs.com/svg.latex?x%20%3A%3D%202kj)

<!-- x := 2kj -->

C++'ta değişkenlerimizi tanımlamak ve takma adlar sağlamak için tür isimlerini kullanabiliriz:

```cpp
auto x = 2 * k * j;
```

Ancak, bu değişebilir ve sadece o anki değerlerin bir anlık görüntüsünü alır. Bazı diller, matematiksel bir tanımlamaya daha yakın olan ön işlemci #define ifadesine sahiptir.

C++'ta daha doğru bir tanımlama şöyle olabilir:

```cpp
const auto x = 2 * k * j;
```

Öte yandan, aşağıdaki ifade eşitliği temsil eder:

![x = 2kj](http://latex.codecogs.com/svg.latex?x%20%3D%202kj)

<!-- x = 2kj -->

Bu denklem de C++'ta öncekiler gibi yorumlanabilir:

```cpp
const auto x = 2 * k * j;
```


## karekök ve karmaşık sayılar

Bir karekök işlemi şu şekildedir:

![squareroot](http://latex.codecogs.com/svg.latex?%5Cleft%28%5Csqrt%7Bx%7D%5Cright%29%5E2%20%3D%20x)

Programlamada, aşağıdaki gibi bir sqrt fonksiyonu kullanırız:

```cpp
    int x = 9;
    std::cout << sqrt(x);
    // 3
```

Kompleks sayılar, ![complex](http://latex.codecogs.com/svg.latex?a&space;&plus;&space;ib) biçimindeki ifadelerdir, ![a](http://latex.codecogs.com/svg.latex?a) gerçek kısım ve ![b](http://latex.codecogs.com/svg.latex?b) de sanal kısımdır. Sanal sayı ![i](http://latex.codecogs.com/svg.latex?i) şöyle tanımlanır:

![imaginary](http://latex.codecogs.com/svg.latex?i%3D%5Csqrt%7B-1%7D).
<!-- i=\sqrt{-1} -->

C++'ta, karmaşık sayılar için yerleşik fonksiyonlar vardır ve bunlarla karmaşık sayı aritmetiği işlemleri yapılabilir. Örneğin:

```cpp
#include <iostream>
#include <complex>

int main()
{
    //{ re: 3, im: -1 }
    std::complex<double> a(3.0, -1.0);
    //{ re: 0, im: 1 }
    std::complex<double> b(0.0, 1.0);
    std::complex<double> c = a * b;

    //'1 + 3i'
    std::cout << c << '\n';

    std::cout << "Gerçek kısım: " << real(c) << '\n';
    std::cout << "Sanal kısım: " << imag(c) << '\n';

    return 0;
}

```

C++'ta karmaşık sayılarla ilgili birçok işlem daha kolayca yapılabilir, bunlar hakkında daha çok bilgi için GeeksforGeeks'teki [Complex numbers in C++](https://www.geeksforgeeks.org/complex-numbers-c-set-1/) yazısına bakabilirsiniz.


## nokta ve çarpı

Nokta `·` ve çarpım `×` sembolleri, içeriğe bağlı olarak farklı kullanımlara sahiptir.

Ne oldukları belli görünebilir, ancak diğer bölümlere devam etmeden önce ince farkları anlamak önemlidir.

#### skaler çarpım

Her iki sembol de skalerlerin basit çarpımını temsil edebilir. Aşağıdakiler eşdeğerdir:

![dotcross1](http://latex.codecogs.com/svg.latex?5%20%5Ccdot%204%20%3D%205%20%5Ctimes%204)

<!-- 5 \cdot 4 = 5 \times 4 -->

Programlama dillerinde çarpma için yıldız işareti kullanma eğilimindeyizdir:

```cpp
int sonuc = 5 * 4
```

Çoğunlukla, çarpım işareti sadece belirsizliği önlemek için kullanılır (ör. iki sayı arasında). Şurada tamamen atlayabiliriz örneğin:

![dotcross2](http://latex.codecogs.com/svg.latex?3kj)

<!-- 3kj -->

Eğer bu değişkenler skalerleri temsil ediyorsa kod şöyle olur:

```cpp
int sonuc = 3 * k * j
```

#### vektör çarpımı

Bir vektörün bir skaler ile çarpımını veya bir vektörün başka bir vektörle öğe öğe çarpımını göstermek için, genellikle nokta `·` veya çarpı `×` sembolleri kullanmayız. Bunlar, lineer cebirde farklı anlamlara sahiptir.

Daha önceki örneğimizi alıp vektörlere uygulayalım. Öğe öğe vektör çarpımını temsil etmek için, Hadamard çarpımında bir açık nokta `∘` kullanıldığını görebilirsiniz.<sup>[2]</sup>

![dotcross3](http://latex.codecogs.com/svg.latex?3%5Cmathbf%7Bk%7D%5Ccirc%5Cmathbf%7Bj%7D)

<!-- 3\mathbf{k}\circ\mathbf{j} -->

Diğer durumlarda yazar, daire içine alınmış bir nokta `⊙` veya dolu bir daire `●` gibi farklı bir gösterimi açıkça tanımlayabilir.<sup>[3]</sup>

2B vektörleri temsil etmek için kodda dizilerin [x, y] nasıl kullanılacağını aşağıda görebilirsiniz:

```cpp
#include <iostream>
#include <array>

std::array<int, 2> carp(std::array<int, 2> a, std::array<int, 2> b)
{
    std::array<int, 2> tmp;
    for(int i = 0; i < a.size(); i++)
        tmp[i] = a[i] * b[i];
    return tmp;
}

std::array<int, 2> skalerCarp(std::array<int, 2> a, int b)
{
    std::array<int, 2> tmp;
    for(int i = 0; i < a.size(); i++)
        tmp[i] = a[i] * b;
    return tmp;
}


int main()
{
    int s = 3;
    std::array<int, 2> k = {1, 2};
    std::array<int, 2> j = {2, 3};

    std::array<int, 2> tmp = carp(k, j);
    std::array<int, 2> sonuc = skalerCarp(tmp, s);

    for(const auto& s: sonuc)
        std::cout << s << ' ';
        // [ 6, 18 ]

    return 0;
}
```

Benzer şekilde, matris çarpımı da genellikle nokta `·` veya çarpı sembolü `×` kullanmaz. Matris çarpımı daha sonraki bölümlerde ele alınacaktır.


#### nokta çarpım

Nokta sembolü `·` iki vektörün [nokta çarpımını](https://www.wikiwand.com/tr/Nokta_%C3%A7arp%C4%B1m) göstermek için kullanılabilir. Skaler olarak değerlendirildiğinden bazen bu skaler çarpım olarak adlandırılır.

![dotcross4](http://latex.codecogs.com/svg.latex?%5Cmathbf%7Bk%7D%5Ccdot%20%5Cmathbf%7Bj%7D)

<!-- \mathbf{k}\cdot \mathbf{j} -->

Lineer cebirin çok yaygın bir özelliğidir ve üç boyutlu bir vektör için aşağıdaki gibi görünebilir:

```cpp
#include <iostream>
#include <array>
#include <numeric>

int main()
{

    std::array<int, 3> k = { 3, -2, 5 };
    std::array<int, 3> j = { -1, -4, 2 };

    std::cout << std::inner_product(std::begin(k), std::end(k), std::begin(j), 0.0);
    //=> 15

    return 0;
}
```

#### çapraz çarpım

Çarpma sembolü `×`, iki vektörün [çapraz çarpımını](https://www.wikiwand.com/tr/%C3%87apraz_%C3%A7arp%C4%B1m) belirtmek için kullanılabilir.

![dotcross5](http://latex.codecogs.com/svg.latex?%5Cmathbf%7Bk%7D%5Ctimes%20%5Cmathbf%7Bj%7D)

<!-- \mathbf{k}\times \mathbf{j} -->

Bu, kodda şöyle görünecektir:

```cpp
#include <iostream>
#include <vector>

template <typename T, typename U>
std::vector<T> crossProduct(std::vector<T> const &a, std::vector<U> const &b)
{
  std::vector<T> r (a.size());
  r[0] = a[1] * b[2] - a[2] * b[1];
  r[1] = a[2] * b[0] - a[0] * b[2];
  r[2] = a[0] * b[1] - a[1] * b[0];
  return r;
}

int main()
{

    std::vector<int> k = { 0, 1, 0 };
    std::vector<int> j = { 1, 0, 0 };

    std::vector<int> sonuc = crossProduct(k, j);

    for(const auto& s: sonuc)
        std::cout << s << ' ';
        //=> [ 0, 0, -1 ]

    return 0;
}
```

Burada, [0, 0, -1] elde ederiz, bu da hem k'nin hem de j'nin dik olduğu anlamına gelir.


## toplam sembolü

Büyük Yunanca `Σ` ([Sigma](https://www.wikiwand.com/tr/Toplam_sembol%C3%BC)), toplama içindir. Başka bir deyişle, bazı sayıları toplar.

![sigma](http://latex.codecogs.com/svg.latex?%5Csum_%7Bi%3D1%7D%5E%7B100%7Di)

<!-- \sum_{i=1}^{100}i -->

Burada `i=1`, 1'den başlanıp toplama sembolünün üstündeki sayı olan 100'de sonlanacağı söylenmektedir. Bunlar sırasıyla alt ve üst sınırlardır. `Σ`'nın sağındaki *i*, bize ne topladığımızı gösterir. Kodda:

```cpp
    int toplam = 0;
    for (int i = 1; i <= 100; i++) {
        toplam += i;
    }
    std::cout << toplam << '\n';
```

Toplamın sonucunu tutan `toplam`'ın değeri 5050 olur.

**İpucu:** Tam sayılarla, bu model aşağıdaki şekilde optimize edilebilir:

```cpp
    int n = 100; // üst sınır
    int toplam = (n * (n + 1)) / 2;

    std::cout << toplam << '\n';
```

İşte, *i*'nin veya "toplanacak şeyin" farklı olduğu başka bir örnek:

![sum2](http://latex.codecogs.com/svg.latex?%5Csum_%7Bi%3D1%7D%5E%7B100%7D%282i&plus;1%29)

<!-- \sum_{i=1}^{100}(2i+1) -->

Kodda:

```cpp
    int toplam = 0;
    for (int i = 1; i <= 100; i++) {
        toplam += (2 * i + 1);
    }
    std::cout << toplam << '\n';
```

Toplamın sonucu 10200'dür.

Notasyon, bir for döngüsünün iç içe olduğuna benzer şekilde iç içe olabilir. Yazar, sırayı değiştirmek için parantez içine almadığı sürece, en sağdaki toplama sembolünü ilk önce değerlendirmelisiniz. Ancak, aşağıdaki durumda, sınırlı miktarlarla uğraştığımız için, sıranın önemi yoktur:

![sigma3](http://latex.codecogs.com/svg.latex?%5Csum_%7Bi%3D1%7D%5E%7B2%7D%5Csum_%7Bj%3D4%7D%5E%7B6%7D%283ij%29)

<!-- \sum_{i=1}^{2}\sum_{j=4}^{6}(3ij) -->

Kodda:

```cpp
    int toplam = 0;
    for (int i = 1; i <= 2; i++) {
        for (int j = 4; j <= 6; j++) {
            toplam += (3 * i * j);
        }
    }

    std::cout << toplam << '\n';
```

veya

```cpp
    int toplam = 0;
    for (int j = 4; j <= 6; j++) {
        for (int i = 1; i <= 2; i++) {
            toplam += (3 * i * j);
        }
    }

    std::cout << toplam << '\n';
```

Burada her iki kod parçası için de `toplam` 135 olacaktır.


## büyük Pi

[Büyük Pi](https://www.wikiwand.com/en/Multiplication#/Capital_Pi_notation), toplama sembolüne çok benzer, tek farkı bir dizi değerin toplamını değil çarpımını bulmak için kullanılır.

Şuna bakalım:

![capitalPi](http://latex.codecogs.com/svg.latex?%5Cprod_%7Bi%3D1%7D%5E%7B6%7Di)

<!-- \prod_{i=1}^{6}i -->

Bu, kodda şöyle görünebilir:

```cpp
    int sonuc = 1;
    for (int i = 1; i <= 6; i++) {
      sonuc *= i;
    }

    std::cout << sonuc << '\n';
```

`sonuc`, 720 olarak hesaplanacaktır.


## borular

Çubuk olarak da bilinen boru sembolleri, içeriğe bağlı olarak farklı şeyler ifade edebilir. Aşağıda üç yaygın kullanım bulunmaktadır: [mutlak değer](https://www.wikiwand.com/tr/Mutlak_de%C4%9Fer), [Öklid normu](https://www.wikiwand.com/en/Norm_(mathematics)#/Euclidean_norm) ve [determinant](https://www.wikiwand.com/tr/Determinant).

Bu üç özelliğin tümü, bir nesnenin uzunluğunu tanımlar.


#### mutlak değer

![pipes1](http://latex.codecogs.com/svg.latex?%5Cleft%20%7C%20x%20%5Cright%20%7C)

<!-- \left | x \right | -->

*x* sayısı için `|x|`, x'in mutlak değerini belirtir. Kodda:

```cpp
#include <iostream>
#include <cmath>

int main()
{
    int x = -5;
    int sonuc = abs(x);
    std::cout << sonuc << '\n';
    // => 5

    return 0;
}
```


#### Öklid normu

![pipes4](http://latex.codecogs.com/svg.latex?%5Cleft%20%5C%7C%20%5Cmathbf%7Bv%7D%20%5Cright%20%5C%7C)

<!-- \left \| \mathbf{v} \right \| -->

**v** vektörü için `‖v‖`, v'nin Öklid normudır. Bu, ayrıca bir vektörün "büyüklüğü" veya "uzunluğu" olarak da adlandırılır.

Çoğunlukla *mutlak değer* gösterimiyle karıştırılmasını önlemek için çift çubukla gösterilir, ancak bazen tek çubukla da gösteriliyor olabilir:

![pipes2](http://latex.codecogs.com/svg.latex?%5Cleft%20%7C%20%5Cmathbf%7Bv%7D%20%5Cright%20%7C)

<!-- \left | \mathbf{v} \right | -->

İşte bir 3B vektörü temsil etmek için [x, y, z] şeklindeki bir dizinin kullanılışına örnek:

```cpp
#include <iostream>
#include <cmath>
#include <vector>

int uzunluk (std::vector<int> vektor) {
  int x = vektor[0];
  int y = vektor[1];
  int z = vektor[2];
  return sqrt(x * x + y * y + z * z);
}

int main()
{
    std::vector<int> v = { 0, 4, -3 };
    std::cout << uzunluk(v) << '\n';
    //=> 5

    return 0;
}
```

Bu işlemi yapmak için C++'ta birden çok yol vardır, bazılarını This Thread'deki [Euclidean norm](http://thisthread.blogspot.com/2012/03/euclidean-norm.html) yazısında görebilirsiniz, buradan alınan bir gerçekleştirim aşağıdadır:

```cpp
#include <iostream>

#include <boost/numeric/ublas/vector.hpp>
#include <boost/numeric/ublas/io.hpp>

int main()
{
    boost::numeric::ublas::vector<int> uv(3);
    uv[0] = 0;
    uv[1] = 4;
    uv[2] = -3;

    std::cout << "Öklid normu: " << boost::numeric::ublas::norm_2(uv) << std::endl;

    return 0;
}
```


#### determinant

![pipes3](http://latex.codecogs.com/svg.latex?%5Cleft%20%7C%5Cmathbf%7BA%7D%20%5Cright%20%7C)

<!-- \left |\mathbf{A}  \right | -->

Bir **A** matrisi için `|A|`, **A** matrisinin determinantı demektir.

Aşağıda, bir 2x2 matrisin determinantını hesaplayan örnek verilmiştir:

```cpp
#include <iostream>
#include <armadillo>

int main()
{
    arma::mat A = "1 0; 0 1;";
    std::cout << arma::det(A) << '\n';

    return 0;
}
```

Yukarıdaki gerçekleştirimde lineer cebir ve bilimsel bilgi işlem kütüphanesi olan [Armadillo](http://arma.sourceforge.net) kullanılmıştır, derleyebilmeniz için başlık dosyasını programınıza dahil etmeli ve örneğin CMake kullanıyorsanız kütüphaneyi `target_link_libraries(${PROJECT_NAME} -larmadillo)` ile programınıza bağlamalısınız.

Eğer bir kütüphane kullanmak yerine determinant alan fonksiyonu kendiniz yazmak isterseniz GeeksforGeeks'teki [Determinant of a Matrix](https://www.geeksforgeeks.org/determinant-of-a-matrix/), Stack Overflow'daki [Fastest way to calculate determinant?](https://stackoverflow.com/questions/36254193/fastest-way-to-calculate-determinant) ve Code Review'daki [Determinant of a matrix](https://codereview.stackexchange.com/questions/79330/determinant-of-a-matrix) yazılarına bakabilirsiniz. Bunlardan sonuncusundan alınmış gerçekleştirim aşağıdadır:

```cpp
#include <iostream>
#include <vector>

static int CalcDeterminant(std::vector<std::vector<int>> Matrix)
   {
        // Bu fonksiyon matrisin determinantını hesaplar
        // herhangi bir boyuttaki matrisin determinantını özyinelemeli olarak hesaplayabilir
        int det = 0; // determinant değeri burada saklanır
        if (Matrix.size() == 1)
        {
            return Matrix[0][0]; // hesaplama gerekmez
        }
        else if (Matrix.size() == 2)
        {
            // Bu durumda, 2 boyutlu matrisin determinantını bir öntanımlı prosedürde hesaplıyoruz
            det = (Matrix[0][0] * Matrix[1][1] - Matrix[0][1] * Matrix[1][0]);
            return det;
        }
        else
        {
            // Bu durumda, 2'den büyük, örneğin 3x3 boyutlarına sahip bir kare matrisin
            // determinantını hesaplıyoruz
            for (int p = 0; p < Matrix[0].size(); p++)
            {
                //this loop iterate on each elements of the first row in the matrix.
                //at each element we cancel the row and column it exist in
                //and form a matrix from the rest of the elements in the matrix
                std::vector<std::vector<int>> TempMatrix; // to hold the shaped matrix;
                for (int i = 1; i < Matrix.size(); i++)
                {
                    // iteration will start from row one cancelling the first row values
                    std::vector<int> TempRow;
                    for (int j = 0; j < Matrix[i].size(); j++)
                    {
                        // iteration will pass all cells of the i row excluding the j
                        //value that match p column
                        if (j != p)
                        {
                           TempRow.push_back(Matrix[i][j]);//add current cell to TempRow
                        }
                    }
                    if (TempRow.size() > 0)
                        TempMatrix.push_back(TempRow);
                    //after adding each row of the new matrix to the vector tempx
                    //we add it to the vector temp which is the vector where the new
                    //matrix will be formed
                }
                det = det + Matrix[0][p] * pow(-1, p) * CalcDeterminant(TempMatrix);
                //then we calculate the value of determinant by using a recursive way
                //where we re-call the function by passing to it the new formed matrix
                //we keep doing this until we get our determinant
            }
            return det;
        }
    };

int main()
{
    std::cout << CalcDeterminant({{1, 0}, {0, 1}}) << '\n';

    return 0;
}
```

## şapka

Geometride, bir karakterin üstündeki "şapka" sembolü, bir [birim vektörünü](https://www.wikiwand.com/tr/Birim_vekt%C3%B6r) temsil etmek için kullanılır. Örneğin, aşağıdaki **a**'nın birim vektörüdür:

![hat](http://latex.codecogs.com/svg.latex?%5Chat%7B%5Cmathbf%7Ba%7D%7D)

<!-- \hat{\mathbf{a}} -->

Kartezyen uzayda, bir birim vektörü tipik olarak 1 uzunluğundadır. Bu, vektörün her bir parçasının -1.0 ila 1.0 aralığında olacağı anlamına gelir. Aşağıda bir 3 boyutlu vektörü bir birim vektörüne *normalize* ediyoruz:

```cpp
#include <iostream>
#include <cmath>
#include <vector>

std::vector<double> normalize(std::vector<double> vec) {
    double x = vec[0];
    double y = vec[1];
    double z = vec[2];
    double squaredLength = x * x + y * y + z * z;

    if (squaredLength > 0) {
        double length = sqrt(squaredLength);
        vec[0] = x / length;
        vec[1] = y / length;
        vec[2] = z / length;
    }
    return vec;
}

int main()
{

    std::vector<double> a = { 0.0, 4.0, -3.0 };

    for(const auto& s: normalize(a))
        std::cout << s << ' ';
        //=> [ 0, 0.8, -0.6 ]

    return 0;
}
```

Benzer bir gerçekleştirim için [2D and 3D vector normalization and angle calculation in C++](http://snipd.net/2d-and-3d-vector-normalization-and-angle-calculation-in-c) yazısına da bakabilirsiniz.


## eleman

Küme teorisinde, bir şeyin bir kümenin elemanı olup olmadığını tanımlamak için "elemanıdır" sembolü `∈` ve `∋` kullanılabilir. Örneğin:

![element1](http://latex.codecogs.com/svg.latex?A%3D%5Cleft%20%5C%7B3%2C9%2C14%7D%7B%20%5Cright%20%5C%7D%2C%203%20%5Cin%20A)

<!-- A=\left \{3,9,14}{  \right \}, 3 \in A -->

Burada `{3, 9, 14}` değerlerinden oluşan bir *A* sayı kümesi var ve diyoruz ki `3`, bu kümenin bir "elemanıdır".

C++'ta basit bir uygulama şöyle olabilir:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main()
{
    int n1 = 8;
    int n2 = 9;

    std::vector<int> vektor = { 3, 9, 14 };

    auto result1 = std::find(std::begin(vektor), std::end(vektor), n1);
    auto result2 = std::find(std::begin(vektor), std::end(vektor), n2);

    if (result1 != std::end(vektor)) {
        std::cout << "vektor'de var: " << n1 << '\n';
    } else {
        std::cout << "vektor'de yok: " << n1 << '\n';
    }

    if (result2 != std::end(vektor)) {
        std::cout << "vektor'de var:: " << n2 << '\n';
    } else {
        std::cout << "vektor'de yok: " << n2 << '\n';
    }

    return 0;
}
```

Bununla birlikte, sadece benzersiz değerleri tutan bir `std::set` kullanmak daha doğru olacaktır:

```cpp
#include <iostream>
#include <algorithm>
#include <set>

int main()
{
    int n1 = 8;
    int n2 = 9;

    std::set<int> sayiKumesi = { 3, 9, 14 };

    // if(sayiKumesi.contains(n1)) { // C++20'de gelecek
    if(sayiKumesi.count(n1) > 0) {
        std::cout << n1 << " var\n";
    } else {
        std::cout << n1 << " yok\n";
    }

    // if(sayiKumesi.contains(n2)) { // C++20'de gelecek
    if(sayiKumesi.count(n2) > 0) {
        std::cout << n2 << " var\n";
    } else {
        std::cout << n2 << " yok\n";
    }

    return 0;
}
```

Geriye doğru olan `∋` de aynıdır, ancak sıra değişir:

![element2](http://latex.codecogs.com/svg.latex?A%3D%5Cleft%20%5C%7B3%2C9%2C14%7D%7B%20%5Cright%20%5C%7D%2C%20A%20%5Cni%203)

<!-- A=\left \{3,9,14}{  \right \}, A \ni 3 -->

Ayrıca `∉` ve `∌` gibi "bir elemanı değildir" sembollerini de kullanabilirsiniz:

![element3](http://latex.codecogs.com/svg.latex?A%3D%5Cleft%20%5C%7B3%2C9%2C14%7D%7B%20%5Cright%20%5C%7D%2C%206%20%5Cnotin%20A)

<!-- A=\left \{3,9,14}{  \right \}, 6 \notin A -->


## yaygın sayı kümeleri

Denklemler arasında bazı büyük [yazı tahtası kalını](https://eksisozluk.com/blackboard-bold--5791356) yazıyüzlü harfler görebilirsiniz. Genellikle, bu kümeleri tanımlamak için kullanılır.

Örneğin, *k*'yi, `ℝ` kümesinin bir elemanı olarak tanımlayabiliriz.

![real](http://latex.codecogs.com/svg.latex?k%20%5Cin%20%5Cmathbb%7BR%7D)

<!-- k \in \mathbb{R} -->

Aşağıda birkaç yaygın küme ve onların sembolleri listelenmiştir.


#### `ℝ` gerçel sayılar

Büyük `ℝ`, gerçel sayı kümesini tanımlar. Bu sayı kümesi tamsayıları, rasyonel sayıları ve irrasyonel sayıları içerir.

C++, kayan nokta sayıları ve tamsayıları aynı tür olarak ele alır, dolayısıyla aşağıdakiler bizim *k* ∈ ℝ örneğimizin basit bir testi olacaktır:

```cpp
#include <iostream>
#include <cmath>

bool isReal (double k) {
    return (!std::isnan(k) && std::isfinite(k));
}

int main()
{
    double n1 = sqrt(-2);

    if (isReal(n1))
        std::cout << n1 << " gercel sayi" << '\n';
    else
        std::cout << n1 << " gercel sayi degil" << '\n';

    return 0;
}
```


#### `ℚ` rasyonel sayılar
Rasyonel sayılar, bir kesir veya *oran* (`⅗` gibi) olarak ifade edilebilen gerçel sayılardır. Rasyonel sayılar payda olarak sıfır alamaz.

Bu aynı zamanda tüm tamsayıların rasyonel sayılar olduğu anlamına gelir, çünkü payda 1 olarak ifade edilebilir.

Öte yandan irrasyonel bir sayı, π (PI) gibi, bir oran olarak ifade edilemeyen bir sayıdır.


#### `ℤ` tam sayılar

Tam sayı, kesirli kısmı olmayan gerçel bir sayıdır. Bunlar pozitif veya negatif olabilir.

C++'ta basit bir test şöyle görünebilir:

```cpp
#include <iostream>
#include <cmath>

bool isInteger (double n) {
    return std::floor(n) == n;
}

int main()
{

    double n1 = 1.0;

    if (isInteger(n1))
        std::cout << n1 << " tam sayı" << '\n';
    else
        std::cout << n1 << " tam sayı degil" << '\n';

    return 0;
}
```

#### `ℕ` doğal sayılar

Doğal bir sayı, pozitif olan ve negatif olmayan bir tam sayıdır. Bağlama ve çalışm alanına bağlı olarak, küme sıfırı içerebilir veya içermeyebilir, bu nedenle şunlardan birine benzeyebilir:

```cpp
{ 0, 1, 2, 3, ... }
{ 1, 2, 3, 4, ... }
```

İlki bilgisayar bilimlerinde daha yaygındır, örneğin:

```cpp
#include <iostream>
#include <cmath>

bool isNaturalNumber (double n) {
  return (std::floor(n) == n && n >= 0);
}

int main()
{
    double n1 = 1.0;

    if (isNaturalNumber(n1))
        std::cout << n1 << " doğal sayı" << '\n';
    else
        std::cout << n1 << " doğal sayı degil" << '\n';

    return 0;
}
```

#### `ℂ` karmaşık sayılar

Karmaşık bir sayı, 2B düzlemde bir koordinat olarak görülen gerçek ve sanal bir sayının birleşimidir. Daha çok bilgi için [A Visual, Intuitive Guide to Imaginary Numbers (Sanal Sayılar için Görsel, Sezgisel Bir Kılavuz)](http://betterexplained.com/articles/a-visual-intuitive-guide-to-imaginary-numbers/)'a bakabilirsiniz.


## fonksiyon

[Fonksiyonlar](https://www.wikiwand.com/tr/Fonksiyon) matematiğin temel özellikleridir ve fonksiyon kavramını koda çevirmek epey kolaydır.

Bir fonksiyon, bir giriş bir çıkış değeriyle ilişkilendirir. Örneğin, aşağıdaki bir fonksiyondur:

![function1](http://latex.codecogs.com/svg.latex?x%5E%7B2%7D)

<!-- x^{2} -->

Bu fonksiyona bir *isim* verebiliriz. Yaygın olarak, bir fonksiyonu tanımlamak için `ƒ` kullanırız, fakat `A(x)` veya başka bir şey de olabilir.

![function2](http://latex.codecogs.com/svg.latex?f%5Cleft%20%28x%20%5Cright%20%29%20%3D%20x%5E%7B2%7D)

<!-- f\left (x  \right ) = x^{2} -->

Kodda `kare` olarak isimlendirip şöyle yazabiliriz:

```cpp
#include <iostream>
#include <cmath>

double kare (double x) {
    return std::pow(x, 2);
}

int main()
{

    double sayi = 4.0;
    std::cout << sayi << " sayısının karesi " << kare(sayi) << " olur.\n";

    return 0;
}
```

Bazen bir fonksiyon adlandırılmaz ve bunun yerine çıktı yazılır.

![function3](http://latex.codecogs.com/svg.latex?y%20%3D%20x%5E%7B2%7D)

<!-- y = x^{2} -->

Yukarıdaki örnekte, *x* giriş, *kare alma* ilişki ve *y* çıkıştır.

Fonksiyonlar, bir programlama dilinde olduğu gibi birden çok parametreye de sahip olabilir. Bunlar matematikte *argümanlar* olarak bilinir ve bir fonksiyonun aldığı argümanların sayısı fonksiyonun *ilişki derecesi* (iki nesne arasındaki ilişki sayısı) olarak bilinir.

![function4](http://latex.codecogs.com/svg.latex?f%28x%2Cy%29%20%3D%20%5Csqrt%7Bx%5E2%20&plus;%20y%5E2%7D)

<!-- f(x,y) = \sqrt{x^2 + y^2} -->

Kodda:

```cpp
double uzunluk (double x, double y) {
  return std::sqrt(x * x + y * y);
}
```

### parçalı fonksiyon

Bazı fonksiyonlar *x* giriş değerine bağlı olarak farklı ilişkiler kullanacaktır.

Aşağıdaki *ƒ* fonksiyonu giriş değerine bağlı olarak iki "alt fonksiyon" arasında seçim yapar.

![piecewise1](http://latex.codecogs.com/svg.latex?f%28x%29%3D%20%5Cbegin%7Bcases%7D%20%5Cfrac%7Bx%5E2-x%7D%7Bx%7D%2C%26%20%5Ctext%7Bif%20%7D%20x%5Cgeq%201%5C%5C%200%2C%20%26%20%5Ctext%7Botherwise%7D%20%5Cend%7Bcases%7D)

<!--    f(x)= 
\begin{cases}
    \frac{x^2-x}{x},& \text{if } x\geq 1\\
    0, & \text{otherwise}
\end{cases} -->

Bu, koddaki `if` / `else` yapısına çok benzer. Sağ taraftaki koşullar genellikle **"for x < 0"** veya **"if x = 0"** olarak yazılır. Durum doğruysa, soldaki işlev kullanılır.

Parçalı fonksiyonlarda, **"aksi halde"** ve **"başka durumlarda"** ifadeleri, koddaki `else` ifadesine benzer.

```cpp
double f (double x) {
    if (x >= 1) {
        return ((std::pow(x, 2) - x) / x);
    } else {
        return 0;
    }
}
```

### yaygın fonksiyonlar

Matematikte her yerde bulunan bazı fonksiyon isimleri vardır. Bir programcı için, bunlar dilde bulunan "yerleşik" fonksiyonlara benzer olabilir.

Böyle bir örnek *sgn* fonksiyonudur. Bu, [işaret fonksiyonu](https://www.wikiwand.com/tr/%C4%B0%C5%9Faret_fonksiyonu) veya diğer adıyla *signum* fonksiyonudur. Bunu tanımlamak için parçalı fonksiyon notasyonunu kullanalım:

![sgn](http://latex.codecogs.com/svg.latex?sgn%28x%29%20%3A%3D%20%5Cbegin%7Bcases%7D%20-1%26%20%5Ctext%7Bif%20%7D%20x%20%3C%200%5C%5C%200%2C%20%26%20%5Ctext%7Bif%20%7D%20%7Bx%20%3D%200%7D%5C%5C%201%2C%20%26%20%5Ctext%7Bif%20%7D%20x%20%3E%200%5C%5C%20%5Cend%7Bcases%7D)

<!-- sgn(x) := 
\begin{cases}
    -1& \text{if } x < 0\\
    0, & \text{if } {x = 0}\\
    1, & \text{if } x > 0\\
\end{cases} -->

Kodda şöyle görünebilir:

```cpp
short sgn (double x) {
    if (x < 0) return -1;
    if (x > 0) return 1;
    return 0;
}
```

veya:

```cpp
template <typename T> int sgn(T val) {
    return (T(0) < val) - (val < T(0));
}
```

[Armadillo](http://arma.sourceforge.net/docs.html#misc_fns), [Boost](https://www.boost.org/doc/libs/1_47_0/libs/math/doc/sf_and_dist/html/math_toolkit/utils/sign_functions.html) gibi çeşitli kütüphaneler aracılığıyla da işaret fonksiyonunu kullanabilirsiniz.

### fonksiyon notasyonu

Literatürde, bazen fonksiyonlar daha açık notasyon ile tanımlanabilir. Örneğin, daha önce bahsettiğimiz `kare` fonksiyona geri dönelim:

![function2](http://latex.codecogs.com/svg.latex?f%5Cleft%20%28x%20%5Cright%20%29%20%3D%20x%5E%7B2%7D)

<!-- f\left (x  \right ) = x^{2} -->

Bu, aşağıdaki biçimde de yazılabilir:

![mapsto](http://latex.codecogs.com/svg.latex?f%20%3A%20x%20%5Cmapsto%20x%5E2)

<!-- f : x \mapsto x^2 -->

Burada bir kuyruğu olan ok, *x, x<sup>2</sup> ile eşleşir* örneğinde olduğu gibi "eşleşir" anlamına gelir.

Bazen, belli olmadığı zaman, notasyon aynı zamanda fonksiyonun *tanım kümesini* ve *değer kümesini* de tanımlayacaktır. *ƒ* fonksiyonunun daha resmi bir tanımı şu şekilde yazılabilir:

![funcnot](http://latex.codecogs.com/svg.latex?%5Cbegin%7Balign*%7D%20f%20%3A%26%5Cmathbb%7BR%7D%20%5Crightarrow%20%5Cmathbb%7BR%7D%5C%5C%20%26x%20%5Cmapsto%20x%5E2%20%5Cend%7Balign*%7D)

<!-- \begin{align*}
f :&\mathbb{R} \rightarrow \mathbb{R}\\
&x \mapsto x^2 
\end{align*}
 -->

Bir fonksiyonun *tanım kümesini* ve *değer kümesini*, sırasıyla *giriş* ve *çıkış* türleri gibidir. İşte, bir tamsayı çıkaran ve önceki *sgn* fonksiyonumuzu kullanan başka bir örnek:

![domain2](http://latex.codecogs.com/svg.latex?sgn%20%3A%20%5Cmathbb%7BR%7D%20%5Crightarrow%20%5Cmathbb%7BZ%7D)

<!-- sgn : \mathbb{R} \rightarrow \mathbb{Z} -->

Buradaki (kuyruğu olmayan) ok bir *kümeyi* bir başka kümeye eşlemek için kullanılır.

JavaScript'te ve dinamik olarak yazılan diğer dillerde, bir fonksiyonun giriş/çıkışını açıklamak ve doğrulamak için belgelendirme ve/veya çalışma zamanı kontrolleri kullanabilirsiniz. Flowtype gibi bazı araçlar, JavaScript'e statik yazımı getirmeye çalışır. Java ve C++ gibi diğer diller, bir fonksiyonun giriş/çıkışının statik türlerine bağlı olarak gerçek fonksiyon aşırı yüklemesine izin verir. Bu, matematiğe daha yakındır: farklı bir tanım kümesi kullanıyorlarsa, iki fonksiyon aynı değildir.


## birincil

[Birincil işareti](https://www.wikiwand.com/en/Prime_(symbol)) (`′`) genellikle, değişken isimlerinde, farklı bir isim vermeden benzer olan şeyleri tanımlamak için kullanılır. Bazı dönüşümlerden sonra "sonraki değeri" tanımlayabilir.

Örneğin, bir 2B noktayı *(x, y)* alıp döndürürsek sonucu *(x′, y′)* olarak adlandırabilirsiniz. Veya **M** matrisinin *transpozesi* **M′** olarak adlandırılabilir.

Kodda, genellikle değişkene, `donusturulmusPozisyon` gibi daha açıklayıcı bir ad atarız.

Bir matematiksel fonksiyon için, birincil işareti genellikle bu fonksiyonun *türevini* tanımlar. Türevler ilerideki bir bölümde açıklanacaktır. Daha önceki bir fonksiyonumuzu ele alalım:

![function2](http://latex.codecogs.com/svg.latex?f%5Cleft%20%28x%20%5Cright%20%29%20%3D%20x%5E%7B2%7D)

<!-- f\left (x  \right ) = x^{2} -->

Bunun türevi bir birincil `′` işaretiyle yazılabilir:

![prime1](http://latex.codecogs.com/svg.latex?f%27%28x%29%20%3D%202x)

<!-- f'(x) = 2x -->

Kodda:

```cpp
template <typename T> T f (T x) {
  return pow(x, 2);
}

template <typename T> T fPrime (T x) {
  return (2 * x);
}
```

İkinci türevi *ƒ′′* ve üçüncü türevi *ƒ′′′* tanımlamak için birden çok birincil işareti kullanılabilir. Bundan sonrakiler için yazarlar genellikle Roma rakamlarını *ƒ*<sup>IV</sup> veya üst simge sayılarını *ƒ*<sup>(n)</sup> kullanır.


## taban ve tavan

Özel parantezler `⌊x⌋` ve `⌈x⌉`, sırasıyla *taban* ve *tavan* fonksiyonlarını temsil eder.

![floor](http://latex.codecogs.com/svg.latex?floor%28x%29%20%3D%20%5Clfloor%20x%20%5Crfloor)

<!-- floor(x) =  \lfloor x \rfloor -->

![ceil](http://latex.codecogs.com/svg.latex?ceil%28x%29%20%3D%20%5Clceil%20x%20%5Crceil)

<!-- ceil(x) =  \lceil x \rceil -->

Kodda:

```cpp
    std::cout << std::ceil(2.4) << '\n'; // 3
    std::cout << std::floor(2.4) << '\n'; // 2
```

İki sembol `⌊x⌉` şeklinde birleştirildiğinde, tipik olarak en yakın tam sayıya yuvarlayan bir fonksiyonu temsil eder:

![round](http://latex.codecogs.com/svg.latex?round%28x%29%20%3D%20%5Clfloor%20x%20%5Crceil)

<!-- round(x) =  \lfloor x \rceil -->

Kodda:

```cpp
    std::cout << std::round(2.4) << '\n'; // 2
```


## oklar

Oklar genellikle fonksiyon notasyonunda kullanılır. Aşağıda görebileceğiniz birkaç farklı alan listelenmiştir.

#### maddi gerektirme
Mantıkta bazen `⇒` ve `→` gibi oklar, *maddi gerektirme* için kullanılır. Yani, A doğruysa, B de doğrudur.

![material1](http://latex.codecogs.com/svg.latex?A%20%5CRightarrow%20B)

<!-- A \Rightarrow B -->

Bunun kod olarak yorumu şöyle olabilir:

```cpp
    bool A = true;
    bool B;
    if (A == true) {
        B = true;
    }
    std::cout << "B: " << B << '\n'; // 1
```

Oklar iki yöne doğru da olabilir `⇐` `⇒` veya iki yönlü de olabilir `⇔`. *A ⇒ B* ve *B ⇒ A* olduğunda aşağıdakine eşdeğer oldukları söylenir:

![material-equiv](http://latex.codecogs.com/svg.latex?A%20%5CLeftrightarrow%20B)

<!-- A \Leftrightarrow B -->

#### eşitlik

Matematikte, `<` `>` `≤` ve `≥`, genellikle bunları kodda kullandığımız şekilde kullanılır, sırasıyla: *daha küçük*, *daha büyük*, *daha küçük veya eşit* veya *daha büyük veya eşit*.

```cpp
    50 > 2 == true;
    2 < 10 == true;
    5 <= 4 == false;
    4 >= 4 == true;
```

Pek sık olmasa da, bu semboller üzerinde *olumsuzluk* anlamı katan bir eğik çizgi görebilirsiniz. Örneğin *k*, "büyük değildir" *j*'den demek için:

![ngt](http://latex.codecogs.com/svg.latex?k%20%5Cngtr%20j)

<!-- k \ngtr j -->

`≪` ve `≫` bazen kaydadeğer bir eşitsizliği temsil etmek için kullanılır. Yani, k'nın, j'den [büyüklük kertesi](https://en.wikipedia.org/wiki/Order_of_magnitude) (basamaksal büyüklük) olarak daha büyük olduğunu ifade eder.

![orderofmag](http://latex.codecogs.com/svg.latex?k%20%5Cgg%20j)

<!-- k \gg j -->

Matematikte, *basamaksal büyüklük* epey spesifiktir; sadece "büyük bir fark" anlamı taşımaz. Yukarıdakilerin basit bir örneği:

```cpp
#include <iostream>
#include <cmath>

double orderOfMagnitude (double n) {
  return std::abs(std::floor(std::log10(std::abs(n))));
}

int main()
{
    double a = 3e-4;
    double b = 2e3;
    if (orderOfMagnitude(a) > orderOfMagnitude(b))
        std::cout << "a'nın basamaksal büyüklüğü (" << orderOfMagnitude(a)
                  <<  "), b'ninkinden (" << orderOfMagnitude(b) << ") büyük.\n";
    else if (orderOfMagnitude(a) < orderOfMagnitude(b))
        std::cout << "a'nın basamaksal büyüklüğü (" << orderOfMagnitude(a)
                  <<  "), b'ninkinden (" << orderOfMagnitude(b) << ") küçük.\n";
    else
        std::cout << "a'nın basamaksal büyüklüğü b'ninkine eşit: "
                  << orderOfMagnitude(a) << '\n';

    return 0;
}
```

#### birleşim ve ayrışım

Okların mantıktaki başka bir kullanımı, birleşim `∧` ve ayrışımdır `∨`. Bunlar sırasıyla programlamadaki `AND` ve `OR` operatörlerine benzemektedir.

Aşağıdaki birleşimi `∧`, mantıksal `AND`'i gösterir:

![and](http://latex.codecogs.com/svg.latex?k%20%3E%202%20%5Cland%20k%20%3C%204%20%5CLeftrightarrow%20k%20%3D%203)

<!-- k > 2 \land k <  4 \Leftrightarrow k = 3   -->

C++’ta &&’i kullanırız. Aşağıdaki örnekte *k*'nin doğal bir sayı olduğu varsayıldığında, mantık *k*'nin 3 olup olmadığını kontrol eder:

```cpp
    int k = 3;
    if (k > 2 && k < 4) {
        std::cout << "k'nın değeri 3'tür: " << k << '\n';
    }
```

Aşağı ok `∨`, OR operatörü gibi mantıksal bir ayrışımdır.

![logic-or](http://latex.codecogs.com/svg.latex?A%20%5Clor%20B)

<!-- A \lor B -->

Kodda:

```cpp
A || B
```


## mantıksal olumsuzluk

Bazen `¬`, `~` ve `!` sembolleri, mantıksal `NOT`'ı temsil etmek için kullanılır. Örneğin, *¬A*, sadece A yanlışsa doğrudur.

*not* sembolünün kullanımına basit bir örnek:

![negation](http://latex.codecogs.com/svg.latex?x%20%5Cneq%20y%20%5CLeftrightarrow%20%5Clnot%28x%20%3D%20y%29)

<!-- x \neq y \Leftrightarrow \lnot(x = y) -->

Bunu kodda nasıl yorumlayabileceğimizin bir örneği de:

```cpp
    int x = 1;
    int y = 2;
    if (x != y) {
        std::cout << "x, y'ye eşit değildir.\n";
    }
```

*Not:* Tilde `~`, içeriğe bağlı olarak birçok farklı anlama sahiptir. Örneğin, satır eşdeğerliği (matris teorisinde) veya aynı basamaksal büyüklük derecesi (eşitlik bölümünde ele alınmıştır) anlamlarına gelebilir.


## aralıklar

Bazen bir fonksiyon, bir dizi değerle sınırlı gerçel sayılarla ilgilenir, böyle bir kısıtlama, bir *aralık* kullanılarak temsil edilebilir.

Örneğin sıfır ve bir arasındaki sayıları, sıfır veya biri dahil ederek veya etmeyerek aşağıdaki gibi temsil edebiliriz:

- Sıfır ve bir dahil değil: ![interval-opened-left-opened-right](http://latex.codecogs.com/svg.latex?%280%2C%201%29)

<!-- (0, 1) -->

- Sıfır dahil ama bir dahil değil: ![interval-closed-left-opened-right](http://latex.codecogs.com/svg.latex?%5B0%2C%201%29)

<!-- [0, 1) -->

- Sıfır dahil değil ama bir dahil: ![interval-opened-left-closed-right](http://latex.codecogs.com/svg.latex?%280%2C%201%5D)

<!-- (0, 1] -->

- Sıfır ve bir dahil: ![interval-closed-left-closed-right](http://latex.codecogs.com/svg.latex?%5B0%2C%201%5D)

<!-- [0, 1] -->

Örneğin, birim küp içinde bir `x` noktasını gösterdiğimizi belirtmek istersek:

![interval-unit-cube](http://latex.codecogs.com/svg.latex?x%20%5Cin%20%5B0%2C%201%5D%5E3)

<!-- x \in [0, 1]^3 -->

C++'ta aralıklar üzerinde çalışmak için [Boost.Icl](https://www.boost.org/doc/libs/1_64_0/libs/icl/doc/html/index.html), [libieeep1788](https://github.com/nehmeier/libieeep1788) ve [Moore](https://www.ime.usp.br/~walterfm/moore/moore.html) kullanılabilir.

Kodda iki elemanlı 1b dizi kullanarak bir aralığı şu şekilde temsil edebiliriz:

```cpp
TODO: cpp'ye çevrilecek
var nextafter = require('nextafter')

var a = [nextafter(0, Infinity), nextafter(1, -Infinity)]     // open interval
var b = [nextafter(0, Infinity), 1]                           // interval closed on the left 
var c = [0, nextafter(1, -Infinity)]                          // interval closed on the right
var d = [0, 1]                                                // closed interval
```

Aralıklar küme işlemleriyle birlikte kullanılır:

- *kesişim*, örneğin: ![interval-intersection](http://latex.codecogs.com/svg.latex?%5B3%2C%205%29%20%5Ccap%20%5B4%2C%206%5D%20%3D%20%5B4%2C%205%29)

<!-- [3, 5) \cap [4, 6] = [4, 5) -->

- *birleşim*, örneğin: ![interval-union](http://latex.codecogs.com/svg.latex?%5B3%2C%205%29%20%5Ccup%20%5B4%2C%206%5D%20%3D%20%5B3%2C%206%5D)

<!-- [3, 5) \cup [4, 6] = [3, 6] -->

- *fark*, örneğin: ![interval-difference-1](http://latex.codecogs.com/svg.latex?%5B3%2C%205%29%20-%20%5B4%2C%206%5D%20%3D%20%5B3%2C%204%29) ve ![interval-difference-2](http://latex.codecogs.com/svg.latex?%5B4%2C%206%5D%20-%20%5B3%2C%205%29%20%3D%20%5B5%2C%206%5D)

<!-- [3, 5) - [4, 6] = [3, 4) -->
<!-- [4, 6] - [3, 5)  = [5, 6] -->

Kodda:

```cpp
TODO: cpp'ye çevrilecek
var Interval = require('interval-arithmetic')
var nextafter = require('nextafter')

var a = Interval(3, nextafter(5, -Infinity))
var b = Interval(4, 6)

Interval.intersection(a, b)
// {lo: 4, hi: 4.999999999999999}

Interval.union(a, b)
// {lo: 3, hi: 6}

Interval.difference(a, b)
// {lo: 3, hi: 3.9999999999999996}

Interval.difference(b, a)
// {lo: 5, hi: 6}
```


## daha çok...

Bu rehberi sevdiniz mi? Daha iyi hale getirmek için [değişiklik isteğinde](https://github.com/maidis/kod-olarak-matematik-cpp/pulls) veya [özellik isteğinde](https://github.com/maidis/kod-olarak-matematik-cpp/issues) bulunmaya ne dersiniz!

## Katkı

Nasıl katkıda bulunacağınıza ilişkin ayrıntılar için, bkz: [CONTRIBUTING.md](./CONTRIBUTING.md).

## Lisans

MIT, detaylar için bkz: [LICENSE.md](./LICENSE.md).

[1]: http://mimosa-pudica.net/improved-oren-nayar.html#images
[2]: http://buzzard.ups.edu/courses/2007spring/projects/million-paper.pdf
[3]: https://www.math.washington.edu/~morrow/464_12/fft.pdf
