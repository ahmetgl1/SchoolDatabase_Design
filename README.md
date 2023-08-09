
# School Database Design

![Ekran Alıntısı](https://github.com/ahmetgl1/SchoolDatabase_Design/assets/81171832/6db0c9c6-3d2b-4c6d-8a47-e45a0724acd3)



<div style="text-align:center;">
  <h2>Veritabanı Oluşturma*</h2>

</div>


```tSQL
CREATE DATABASE SchoolDatabase
ON PRIMARY
(
    NAME = SchoolDatabase,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\DATA\SchoolDatabase.mdf'
)
LOG ON
(
    NAME = SchoolDatabase_log,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\DATA\SchoolDatabase_log.ldf'
)
```
- "**SchoolDatabase**" adında bir veritabanı oluşturuldu.

Datalar, belirtilen dosya yolunda "SchoolDatabase.mdf" ve "SchoolDatabase_log.ldf" dosyalarında depolanacaktır.
*(log yedekleme veya silinme gibi durumlarda kullanmak için)*
                                                

   **TABLOLAR**

```tSQL
GO
CREATE TABLE [dbo].[Users](
	[Id] [uniqueidentifier] NOT NULL,
	[Name] [varchar](70) NOT NULL,
	[Surname] [varchar](50) NOT NULL,
	[Birthdate] [date] NOT NULL,
	[Gender] [char](1) NOT NULL,
	[PhoneNumber] [char](11) NULL,
	[Email] [varchar](200) NULL,
	[CreatetedDate] [date] NOT NULL,
	[UpdatedDate] [date] NULL,
	[TittleId] [int] NULL,
 CONSTRAINT [PK_Users_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```

Buradaa,

- **Id:** Her bir kullanıcı kaydı için benzersiz bir tanımlayıcı GUID veri tipindendir *(Primary Key)*  
- **Name:** Kullanıcının adını belirtir.  **VARCHAR(70)** veri tipine sahiptir *NOT NULL*
- **Surname:** Kullanıcının soyadını belirtir. **VARCHAR(50)** veri tipine sahiptir*NOT NULL*
- **Birthdate:** Kullanıcının doğum tarihini belirtir. **DATE** veri tipine sahiptir *NOT NULL*
- **Gender:** Kullanıcının cinsiyetini belirtir. **CHAR(1)** veri tipine sahiptir *NOT NULL*
- **PhoneNumber:** Kullanıcının telefon numarasını belirtir. **CHAR(11)** veri tipine sahiptir *boş bırakılabilir* 
- **Email:** Kullanıcının e-posta adresini belirtir. **VARCHAR(200) veri tipine sahiptir *boş bırakılabilir* 
- **CreatetedDate:** Kullanıcının oluşturulma tarihini belirtir. **DATE** veri tipine sahiptir *NOT NULL*
- **UpdatedDate:** Kullanıcının bilgilerinin güncellendiği tarihi belirtir. Bu sütun date veri tipine sahiptir *boş bırakılabilir*  
- **TittleId:** Kullanıcının  **title**'ı belirtir. **INT** veri tipine sahiptir *boş bırakılabilir* 
  ! "Titles" tablosundaki bir unvan kaydının *"Id"* değerine referans verir.



#2.1 Students#
```tSQL 
GO
CREATE TABLE [dbo].[Students](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[UserId] [uniqueidentifier] NOT NULL,
	[ClassId] [int] NOT NULL,
	[ScholarshipRate] [decimal](5, 2) NOT NULL,
	[EnrollmentDate] [date] NOT NULL,
	[ProgramId] [int] NOT NULL,
 CONSTRAINT [PK_Students_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```

 "Ogrenci" adında bir tablo oluşturuldu.  Öğrenci bilgilerini içerir ve *"ClassId"* sütunu ile *"ProgramId"* (foreign key)   ilişkisi kurar.

- Id: Her öğrenci için benzersiz (Primary Key)
- UserId: Her öğrenci için eşsiz bir kimlik numarası.
- ClassId: Her öğrencinin ait olduğu sınıfın numarası.
- ScholarshipRate: Öğrencinin burs oranı.
- EnrollmentDate: Öğrencinin kaydolma tarihi.
- ProgramId: Öğrencinin Program numarası 
```tSQL
GO
CREATE TABLE [dbo].[Classes](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[ClassName] [varchar](10) NULL,
 CONSTRAINT [PK_Classes_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```
*"Classes"* adında başka bir tablo oluşturudu.

- Id: Her sınıf için benzersiz bir numara(Primary Key)
- ClassName: Sınıfın adı, metin formatında.
Aynı zamanda Id sütunu birincil anahtar olarak ayarlanır *(Primary Key)*. 

``` tSQL
GO
CREATE TABLE [dbo].[Tourniques](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[UserId] [uniqueidentifier] NOT NULL,
	[LocationId] [int] NOT NULL,
	[EntryDate] [date] NOT NULL,
	[ExistDate] [date] NOT NULL,
	[isInside] [bit] NOT NULL,
 CONSTRAINT [PK_Tourniques_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
``` 

*"Tourniques"* adında yeni bir tablo oluşturuldu.

- Id: Her Turnike noktası için benzersiz *(Primary Key)*.
- UserId: Kullanıcının benzersiz tanımlayıcı numarası(Unique Key).
- LocationId: Turnike noktasının konumunu belirten bir sayı.
- EntryDate: Kullanıcının Turnike noktasına giriş yaptığı tarih.
- ExistDate: Kullanıcının Turnike noktasından çıkış yaptığı tarih.
- isInside: Kullanıcının Turnike noktası içinde olup olmadığını belirten bir bit  değeridir.

! Id sütunu Primary Key anahtar olarak ayarlanır ve Turnikenin Id değeri ile ilişkilidir. 

```tSQL
GO
CREATE TABLE [dbo].[Addresses](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[CountryId] [tinyint] NOT NULL,
	[CityId] [tinyint] NOT NULL,
	[TownId] [tinyint] NOT NULL,
	[UserId] [uniqueidentifier] NOT NULL,
 CONSTRAINT [PK_Addresses_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY],
 CONSTRAINT [IX_UserId_Unique] UNIQUE NONCLUSTERED 
(
	[UserId] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
``` 
**"Addresses"** adlı yeni bir tablo oluşturuldu

- **Id:** Her bir adres kaydı için benzersiz *(Primary Key)*
- **CountryId:** Ülkenin benzersiz tanımlayıcı numarasını içeren en fazla 255 değer alan **TINYINT** veri tipi.
- **CityId:** Şehrin benzersiz tanımlayıcı numarasını içeren en fazla 255 değer alan **TINYINT** veri tipi.
- **TownId:** İlçenin benzersiz tanımlayıcı numarasını içeren en fazla 255 değer alan **TINYINT** veri tipi.
- **UserId:** Adresin sahibi olan kullanıcının benzersiz tanımlayıcı numarası.
 Id sütunu Primary Key  olarak ve *UserId* sütunu da benzersiz bir değer olarak ayarlanmıştır. Yni her adres kaydının benzersiz bir tanımlayıcıya sahip olduğunu ve her kullanıcının yalnızca bir adres kaydına sahip olabileceğini belirtir.  *"Addresses"* adıyla oluşturulur ve yukarıda belirtilen sütunlar, birincil anahtar ve benzersiz şekilde birlikte  gelir.


```tSQL
GO
CREATE TABLE [dbo].[Assistans](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[EmployeeId] [int] NOT NULL,
	[TeacherId] [int] NOT NULL,
 CONSTRAINT [PK_Assistans_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
 ```
 **"Assistans"** adlı bir tablo oluşturuldu

- **Id:** Her bir asistan kaydı için benzersiz bir tanımlayıcı *(Primary Key)*
- **EmployeeId:** Asistan olarak atanmış çalışanın tanımlayıcı numarasını içeren **INT** veri tipindedir.
- **TeacherId:** Yardımcı olarak atanmış öğretmenin tanımlayıcı numarasını içeren **INT** veri tipindedir.
Id *Birincil Anahtar* olarak oluşturuldu.  
-
```tSQL
GO
CREATE TABLE [dbo].[Cities](
	[Id] [tinyint] IDENTITY(1,1) NOT NULL,
	[CountryId] [tinyint] NOT NULL,
	[CityName] [varchar](80) NOT NULL,
 CONSTRAINT [PK_Cities_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
 ```
 **"Cities"** adlı bir tablo oluşturuldu. 

- **Id:** Her bir şehir kaydı için benzersiz bir tanımlayıcı numara. En fazla 255 değer alan **TINYINT**  veri tipine sahiptir.
- **CountryId:** Şehrin bulunduğu ülkenin tanımlayıcı numaraEn fazla 255 değer alan **TINYINT**  veri tipine sahiptir. 
- **CityName:** Şehir adını içeren  **VARCHAR** veri tipindedir. Şehrin adı burada saklanır.*(İstanbul, Ankara ...)*
Id sütunu *Primary Key* olarak oluşturuldu.  *"Cities"* adıyla oluşturuldu ve yukarıda belirtilen sütunlar ve Primary Key  ile birlikte gelir ve tabloda ait bilgiler saklanır.

-
```tSQL
CREATE TABLE [dbo].[Countries](
	[Id] [tinyint] IDENTITY(1,1) NOT NULL,
	[CountryName] [varchar](80) NOT NULL,
 CONSTRAINT [PK_Countries_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```
**"Countries"** adlı bir tablo oluşturulduldu.

**-Id:** Her bir ülke kaydı için *Primary Key*  En fazla 255 değer alan **TINYINT**  veri tipine sahiptir 
**-CountryName:** Ülkenin adını içeren bir VARCHAR alanıdır.
*Id* sütunu birincil anahtar olarak ayarlanoluşturuldu  *"Countries"* adıyla oluşturuldu
-
```tSQL
GO
CREATE TABLE [dbo].[CourseRegistrations](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[StudentId] [int] NOT NULL,
	[CourseId] [int] NOT NULL,
	[RegistrationDate] [date] NOT NULL,
 CONSTRAINT [PK_CourseRegistrations_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```


**"CourseRegistrations"** adlı bir tablo oluşturuldu

- **Id:** Her bir kurs kaydı için benzersiz bir tanımlayıcı *(Primary Key)*. INT veri tipine sahiptir 
- **StudentId:** Kurs kaydının hangi öğrenciye ait olduğunu belirten referans bir alandır. **INT** veri tipine sahiptir ve öğrenciye ait olan kaydın *StudentId*'sini içerir.
- **CourseId:** Hangi kursa kaydın yapıldığını belirten referans  alanıdır.  **INT** veri tipine sahiptir
- **RegistrationDate:** Kurs kaydının yapıldığı tarih. **DATE** veri tipine sahiptir.
! Id sütunu birincil anahtar olarak oluşturuldu. 



-
```tSQL
GO
CREATE TABLE [dbo].[Courses](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[Credit] [tinyint] NOT NULL,
	[TeacherId] [int] NOT NULL,
	[CourseName] [varchar](50) NULL,
 CONSTRAINT [PK_Courses_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

```
**"Courses"** adlı bir tablo oluşturuldu

 - **Id:** Her bir Dersin kaydı için benzersiz bir tanımlayıcı *PrimeyKey*. **INT** veri tipine sahiptir
 - **Credit:** Dersin kredi değeri. En fazla 255 değer alan **TINYINT**  veri tipine sahiptir

 - **CourseName:** Dersin adı. En fazla 50 karakter uzunluğunda **VARCHAR** veri tipine sahiptir

-

```tSQL
GO
CREATE TABLE [dbo].[Departments](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[DepartmentName] [varchar](70) NOT NULL,
	[HeadOfDepartmentId] [int] NOT NULL,
 CONSTRAINT [PK_Departments_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

```

 **"Departments"** adlı bir tablo oluşturuldu

- **Id:** Her bir bölüm kaydı için benzersiz bir tanımlayıcıdır.  **INT**  veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
- **DepartmentName:** Bölümün adı. En fazla 70 karakter uzunluğunda  **VARCHAR** veri tipine sahiptir.
- **HeadOfDepartmentId:** Bölüm başkanına atanan referans bir alan. INT veri tipine sahiptir ve ilgili bölümün başkanının Id'sini içerir.

! Id sütunu birincil anahtar olarak oluşturuldu. *"Departments"* adıyla oluşturuldu 

-
```tSQL
GO
CREATE TABLE [dbo].[Employees](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[UserId] [uniqueidentifier] NOT NULL,
	[HireDate] [date] NOT NULL,
	[LeavingDate] [date] NULL,
 CONSTRAINT [PK_Employees_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

```
**Employees"** adlı bir tablo oluşturuldu

- **Id:** Her bir çalışan kaydı için benzersiz bir tanımlayıcı *(Primary Key)*. **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
- **UserId:** Çalışana atanmış benzersiz bir tanımlayıcıdır **uniqueidentifier** veri tipine sahiptir.
- **HireDate:** Çalışanın işe alındığı tarih. **DATE** veri tipine sahiptir ve zorunlu bir alandır.
- **LeavingDate:** Çalışanın ayrılma tarihi. **DATE** veri tipine sahiptir ancak boş bırakılabileceği için **NULL** değeri alabilir.

! Id sütunu birincil anahtar olarak ayarlanmıştır. Tablo, "Employees" adıyla oluşturulur ve yukarıda belirtilen sütunlar ve birincil anahtar ile birlikte gelir.

-
```tSQL
GO
CREATE TABLE [dbo].[Examss](
	[Id] [tinyint] IDENTITY(1,1) NOT NULL,
	[ExamName] [varchar](20) NOT NULL,
 CONSTRAINT [PK_Examss] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```
**"Examss"** adlı bir tablo oluşturuldu

- **Id:** Her bir sınav kaydı için benzersiz bir tanımlayıcı *(Primary Key)* En fazla 255 değer alan **TINYINT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak oluşturuldu
- **ExamName:** Sınavın adı  **VARCHAR** veri tipine sahiptir ve zorunlu bir alandır.

Id sütunu birincil anahtar olarak oluşturuldu. Tablo, **"Examss"** adıyla oluşturulur ve yukarıda belirtilen sütunlar ve birincil anahtar ile birlikte gelir. farklı sınav türlerini veya isimlerini saklamak için kullanılabilir.

-
```tSQL
GO
CREATE TABLE [dbo].[Graduates](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[GraduatedDate] [date] NULL,
	[StudentId] [int] NOT NULL,
 CONSTRAINT [PK_Graduates_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```
**"Graduates"** adlı bir tablo oluşturuldu

- **Id:** Her bir mezun kaydı için benzersiz bir tanımlayıcı numara *(Primary Key)*. Bu sütun **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
 - **GraduatedDate:** Mezuniyet tarihi. Bu sütun **DATE** veri tipine sahiptir ancak **NULL** değer alabilir.
- **StudentId:** Mezun olan öğrencinin Id'sini belirtir. Bu sütun int veri tipine sahiptir ve zorunlu bir alandır.
Id sütunu birincil anahtar olarak oluşturuldu.  Öğrencilerin mezuniyet bilgilerini tutmak için kullanılabilir. Her bir mezuniyet kaydı, öğrencinin mezuniyet tarihi ve öğrenci kimliği gibi bilgileri içerir.

-
```tSQL
GO
CREATE TABLE [dbo].[HeadOfDepartments](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[TeacherId] [int] NOT NULL,
 CONSTRAINT [PK_HeadOfDepartments_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

```
**"HeadOfDepartments"** adlı bir tablo oluşturuldu

- **Id:** Her bir bölüm başkanı kaydı için benzersiz bir tanımlayıcı numara *(Primary Key)*. Bu sütun **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
 - **TeacherId:** Bölüm başkanı olarak atanmış öğretmenin Id'sini belirtir. **INT** veri tipine sahiptir ve zorunlu bir alandır.
*Id* sütunu birincil anahtar olarak oluşturulu. Bu tablo, bölümlerin başkanlarını ve bu başkanların öğretmen kimliklerini ilişkilendirmek için kullanılabilir.
-
```tSQL
GO
CREATE TABLE [dbo].[Locations](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[LocationName] [varchar](70) NOT NULL,
 CONSTRAINT [PK_Locations_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```

 **"Locations"** adlı bir tablo oluşturuldu

- **Id:** Her bir konum kaydı için benzersiz bir tanımlayıcı numara(Primary Key). **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
 - **LocationName:** Konumun adını belirtir. **VARCHAR(70)** veri tipine sahiptir ve zorunlu bir alandır.
Id sütunu birincil anahtar olarak oluşturuldu. farklı konumları belirtmek için kullanıldı.

```tSQL
GO
CREATE TABLE [dbo].[Managers](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[EmployeeId] [int] NOT NULL,
 CONSTRAINT [PK_Managers_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

```
**"Managers"** adlı bir tablo oluşturuldu.

 - **Id:** Her bir yönetici kaydı için benzersiz bir tanımlayıcı numara *(PrimaryKey)* . Bu sütun **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
- **EmployeeId:** Yönetici olarak atanmış olan çalışanın Id'sini belirtir. Bu sütun **INT** veri tipine sahiptir ve zorunlu bir alandır.
 Id sütunu birincil anahtar olarak oluşturuldu. Farklı çalışanların yöneticilerini belirtmek için kullanılabilir.

```tSQL
GO
CREATE TABLE [dbo].[Notes](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[StudentId] [int] NOT NULL,
	[CourseId] [int] NOT NULL,
	[ExamTypeId] [tinyint] NOT NULL,
	[Point] [char](2) NULL,
 CONSTRAINT [PK_Notes] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

```
**"Notes"** adlı bir tablo oluşturuldu.

- **Id:** Her bir not kaydı için benzersiz bir tanımlayıcı *(PrimaryKey)* . **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
 - **StudentId:** Notun hangi öğrenciye ait olduğunu belirtir. **INT** veri tipine sahiptir ve zorunlu bir alandır.
- **CourseId:** Notun hangi dersle ilişkili olduğunu belirtir. **INT** veri tipine sahiptir ve zorunlu bir alandır.
- **ExamTypeId:** Notun hangi türde bir sınav sonucu olduğunu belirtir. **TINYINT** veri tipine sahiptir ve zorunlu bir alandır.
- **Point:** Öğrencinin aldığı puanı temsil eder. **CHAR(2)** veri tipine sahiptir ve **NULL** değer alabilir.
Id sütunu birincil anahtar olarak oluşturuldu

```tSQL
GO
CREATE TABLE [dbo].[Parents](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[UserId] [uniqueidentifier] NOT NULL,
	[StudentId] [int] NOT NULL,
 CONSTRAINT [PK_Parents_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```
**"Parents"** adlı bir tablo oluşturuldu

- **Id:** Her bir veli kaydı için benzersiz bir tanımlayıcı *(PrimaryKey)* . **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
- **UserId:** Ebeveynin kullanıcı kimliğini temsil eder. **UNIQUEIDENTIFIER** veri tipine sahiptir ve zorunlu bir alandır.
- **StudentId:** Ebeveynin hangi öğrenciye ait olduğunu belirtir.**INT** tipine sahiptir ve zorunlu bir alandır.


```tSQL
GO
CREATE TABLE [dbo].[Programs](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[ProgramName] [varchar](50) NOT NULL,
 CONSTRAINT [PK_Programs_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```
**"Programs"** adlı bir tablo oluşturuldu

- **Id:** Her bir program kaydı için benzersiz bir tanımlayıcı *(PrimaryKey)* . **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
 - **ProgramName:** Programın adını temsil eder. **INT** veri tipine sahiptir ve zorunlu bir alandır.
 
!Id sütunu birincil anahtar olarak ayarlanmıştır.

```tSQ
GO
CREATE TABLE [dbo].[Salariesss](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[Amount] [money] NOT NULL,
	[EmployeeId] [int] NOT NULL,
 CONSTRAINT [PK_Salariesss] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```
**"Salariesss"** adlı bir tablo oluşturuldu

 - **Id:** Her bir maaş kaydı için benzersiz bir tanımlayıcı *(PrimaryKey)*  **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
 - **Amount:** Çalışanın maaş miktarını temsil eder. **MONEY** veri tipine sahiptir ve zorunlu bir alandır.
- **EmployeeId:** Maaş kaydının hangi çalışana ait olduğunu belirtir. INT veri tipine sahiptir ve zorunlu bir alandır.

!Id sütunu birincil anahtar olarak ayarlanmıştır. 

```tSQL
GO
CREATE TABLE [dbo].[Staffs](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[EmployeeId] [int] NOT NULL,
 CONSTRAINT [PK_Staffs_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```
 **"Staffs"** adlı bir tablo oluşturuldu

 - **Id:** Her bir personel kaydı için benzersiz bir tanımlayıcı *(PrimaryKey)*. Bu sütun **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
- **EmployeeId:** Personelin kimliğini belirtir. **INT** veri tipine sahiptir ve zorunlu bir alandır.

 !Id sütunu birincil anahtar olarak ayarlanmıştır.


```tSQL
GO
CREATE TABLE [dbo].[Teachers](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[ClassId] [int] NOT NULL,
	[EmployeeId] [int] NOT NULL,
 CONSTRAINT [PK_Teachers_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

```
**"Teachers"** adlı bir tablo oluşturuldu

- **Id:** Her bir öğretmen kaydı için benzersiz bir tanımlayıcı *(PrimaryKey)* . **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
- **ClassId:** Öğretmenin atanmış olduğu sınıfın kimliğini belirtir. **INT** veri tipine sahiptir ve zorunlu bir alandır.
- **EmployeeId:** Öğretmenin kimliğini belirtir. **INT** veri tipine sahiptir ve zorunlu bir alandır.

! Id sütunu birincil anahtar olarak ayarlanmıştır

```tSQL
GO
CREATE TABLE [dbo].[Titles](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[TittleName] [varchar](60) NOT NULL,
 CONSTRAINT [PK_Titles] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```

 **"Titles"** adlı bir tablo oluşturudu

- **Id:** Her bir unvan kaydı için benzersiz bir tanımlayıcı *(PrimaryKey)* . **INT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
- **TitleName:** Unvanın adını belirtir. Bu sütun **VARCHAR(60)** veri tipine sahiptir ve zorunlu bir alandır.
 
! Id sütunu birincil anahtar olarak ayarlanmıştır.

```tSQL
GO
CREATE TABLE [dbo].[Towns](
	[Id] [tinyint] IDENTITY(1,1) NOT NULL,
	[CityId] [tinyint] NOT NULL,
	[TownName] [varchar](70) NOT NULL,
 CONSTRAINT [PK_Towns_] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

```

**"Towns"** adlı bir tablo oluşturuldu

 - **Id:** Her bir Ilçe kaydı için benzersiz bir tanımlayıcı *(PrimaryKey)*. **TINYINT** veri tipine sahiptir ve otomatik artan bir kimlik sütunu olarak ayarlanmıştır.
- **CityId:** ilçenin hangi şehre  ait olduğunu belirtir. **TINYINT** veri tipine sahiptir ve zorunlu bir alandır. 
   "Cities" tablosundaki bir şehir kaydının *"Id"* değerine referans verir.
 - **TownName:** Kasabanın adını belirtir.**VARCHAR(70)** veri tipine sahiptir ve zorunlu bir alandır.

   !Id sütunu birincil anahtar olarak ayarlanmıştır 


**Procedure**
```tSQL
GO

create PROCEDURE [dbo].[CreateUsersProc]
    @Name VARCHAR(70),
    @Surname VARCHAR(50),
    @Birthdate DATE,
    @Gender CHAR(1),
    @PhoneNumber CHAR(11),
    @Email VARCHAR(200),
    @TittleName VARCHAR(60)
AS
BEGIN
    DECLARE @TitleId INT;
    SELECT @TitleId = Id FROM Titles WHERE Titles.TittleName = @TittleName;

    DECLARE @UserId UNIQUEIDENTIFIER = NEWID();
 --    SET @UserId = SCOPE_IDENTITY();

    INSERT INTO Users(Id, Name, Surname, Birthdate, Gender, PhoneNumber, Email, CreatetedDate, UpdatedDate, TittleId)
    VALUES (NEWID(), @Name, @Surname, @Birthdate, @Gender, @PhoneNumber, @Email, GETDATE(), NULL, @TitleId);
END;
GO
``` 
- Burada kullanıcıdan istenilen veriler girilir.Bu sayede  Employee Teacher, Student gibi bilgileri her birinde yazmak yerine
bir defa yazılıp diğer bilgiler için kullanılır.

```tSQL
GO
CREATE PROCEDURE [dbo].[RegisterEmployees]

  @Name VARCHAR(70),
  @Surname VARCHAR(50),
  @Birthdate DATE,
  @Gender CHAR(1),
  @PhoneNumber CHAR(11),
  @Email VARCHAR(200),
  @TittleName VARCHAR(60),
  @HireDate DATE,
  @LeavingDate DATE 

  AS

  BEGIN 
  
  DECLARE @UserId  UNIQUEIDENTIFIER  = NEWID();

  EXEC CreateUsersProc
               @Name,
               @Surname,
               @Birthdate,
               @Gender,
               @PhoneNumber,
               @Email,
               @TittleName 


	

	


 SELECT @UserId = Id FROM Users WHERE Email = @Email;

 INSERT INTO Employees( UserId , HireDate , LeavingDate) VALUES
 (@UserId ,  @HireDate , @LeavingDate)
 

 END
GO
```
```tSQL

GO
CREATE PROCEDURE [dbo].[RegisterEmployees]

  @Name VARCHAR(70),
  @Surname VARCHAR(50),
  @Birthdate DATE,
  @Gender CHAR(1),
  @PhoneNumber CHAR(11),
  @Email VARCHAR(200),
  @TittleName VARCHAR(60),
  @HireDate DATE,
  @LeavingDate DATE 

  AS

  BEGIN 
  
  DECLARE @UserId  UNIQUEIDENTIFIER  = NEWID();

  EXEC CreateUsersProc
               @Name,
               @Surname,
               @Birthdate,
               @Gender,
               @PhoneNumber,
               @Email,
               @TittleName 


	

	


 SELECT @UserId = Id FROM Users WHERE Email = @Email;

 INSERT INTO Employees( UserId , HireDate , LeavingDate) VALUES
 (@UserId ,  @HireDate , @LeavingDate)
 

 END
GO
```

**VIEW**
```tSQL

GO

CREATE VIEW [dbo].[StudntInformation] 

AS

SELECT Users.Name, Users.Surname,   Students.EnrollmentDate, Classes.ClassName FROM Users
JOIN Students ON Users.Id = Students.UserId
JOIN Classes ON Classes.Id = Students.ClassId

GO

 ```

 Turnikeleri kontrol amacı ile hangi kullanıcının okulda olup olmadığını görüntülemek için kullanılan **View**
```tSQL

GO
CREATE VIEW [dbo].[StudntInformation] 

AS

SELECT Users.Name, Users.Surname,   Students.EnrollmentDate, Classes.ClassName FROM Users
JOIN Students ON Users.Id = Students.UserId
JOIN Classes ON Classes.Id = Students.ClassId

GO

```
- Öğrencilerin ismi, soyismi, kayıt tarihlerini ve sınıflarını görüntülemek için oluşturulan sanal tablo **VIEW**





