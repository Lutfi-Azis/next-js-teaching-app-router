# Komponen Server dan Client

## Environments

![Client boundary server](assets/client-boundary-server.webp)

Terdapat dua environment suatu komponen bisa dirender: di client dan di server. Client merujuk pada browser user yang mengunjungi situs Next JS. Sementara server merujuk pada komputer yang menjalankan framework Next JS kita.

Kedua environment ini memiliki perbedaan dalam fungsionalitas.

Komponen yang dirender di server bisa

- mengakses dependencies backend (seperti Prisma, TypeORM, Node API) secara langsung;
- dan mengakses informasi rahasia (seperti API key, dan access tokens).

Sementara komponen yang dirender di cient bisa

- menambahkan interaktivitas dan event listeners (seperti `onClick`);
- menggunakan state dan lifecycle effects (seperti `useState`, `useReducer`, `useEffect`);
- dan menggunakan API yang hanya ada di browser (seperti objek `window`).

## Server component

Komponen server dirender di environment server. Next JS menggunakan server component secara default.

## Client component

Komponen server bisa di pre-render dan dijalankan (melalui SSR) di server, dan dihidrasi di client. Jadi perlu diingat, client component itu akan dijalankan di server untuk pre-rendering (SSR), dan di client untuk rendering.

## Penggunaan

Gunakan server component jika Anda ingin

- menge-fetch data;
- mengakses informasi rahasia di server;
- mengakses dependencies backend (seperti Prisma, TypeORM, Node API) secara langsung;
- dan menyimpan dependencies berukuran besar di server agar tidak dijalankan di client untuk meringankan ukuran javascript yang dikirim ke client.

Gunakan client component jika Anda ingin

- menambahkan interaktivitas dan event listeners (seperti `onClick`);
- menggunakan state dan lifecycle effects (seperti `useState`, `useReducer`, `useEffect`);
- dan menggunakan API yang hanya ada di browser (seperti objek `window` dan Geolocation API).

# Perbedaan App Router dengan Page Router

Next.js memiliki dua pendekatan utama dalam mengelola routing aplikasi: **App Router** dan **Page Router**. Kedua sistem ini memiliki tujuan dan karakteristik yang berbeda. Berikut adalah penjelasan lengkapnya.

## 1. Page Router (Dirilis Sebelum Next.js 13)

- **Struktur Folder**: `pages/`
- **Routing Berdasarkan Folder**: Semua file di dalam folder `pages/` otomatis menjadi rute berdasarkan struktur filenya. Misalnya, file `pages/about.js` akan diakses melalui URL `/about`.
- **Server-Side Rendering (SSR) dan Static Generation**:
  - Mendukung server-side rendering melalui `getServerSideProps`.
  - Mendukung static generation melalui `getStaticProps` dan `getStaticPaths`.
- **Custom API Routes**: API routes juga dibuat di dalam folder `pages/api`, misalnya `pages/api/hello.js` akan diakses di `/api/hello`.
- **Penggunaan Hooks**: Mendukung hooks React seperti `useEffect`, tetapi tidak mendukung fitur server components.

## 2. App Router (Dirilis di Next.js 13)

- **Struktur Folder**: `app/`
- **Server Components dan Client Components**:
  - Di App Router, komponen dapat diklasifikasikan sebagai **Server Component** atau **Client Component**.
  - Defaultnya, semua komponen di dalam `app/` adalah **Server Component**. Jika komponen perlu dijalankan di klien, kita harus menggunakan `"use client";` di bagian atas file tersebut.
- **File-Based Routing yang Lebih Kompleks**:
  - Menggunakan file `layout.js`, `page.js`, dan `error.js` untuk mengatur struktur halaman.
  - Halaman utama biasanya berada di `app/page.js`, sedangkan sub-rute bisa ditambahkan di dalam folder dengan nama yang sesuai, misalnya `app/about/page.js` untuk `/about`.
- **Pengaturan Layout**:
  - Menggunakan file `layout.js` untuk membuat layout yang persist (tetap ada) di seluruh halaman, seperti header atau footer.
  - Layouts dapat di-nest (bersarang), memungkinkan struktur yang lebih kompleks.
- **Data Fetching**:
  - Menggunakan fungsi async secara langsung dalam komponen untuk pengambilan data.
  - Dukungan penuh untuk `Suspense` dan streaming data, yang memberikan pengalaman loading yang lebih cepat dan interaktif.
- **Dynamic Segments**:
  - Mendukung **dynamic segments** untuk rute dinamis dengan struktur folder seperti `[id]`.
- **API Routes**: Mendukung API routes, tetapi tidak perlu terikat pada folder `pages/api` seperti pada Page Router.

## Tabel Perbedaan Singkat

| Fitur                   | Page Router             | App Router               |
|-------------------------|-------------------------|--------------------------|
| Struktur Folder         | `pages/`                | `app/`                   |
| Default Component       | Client Components       | Server Components        |
| Data Fetching           | `getStaticProps`, `getServerSideProps` | Fetching langsung di komponen |
| Dukungan API Routes     | `pages/api/`            | Di dalam `app/`          |
| Layout Management       | Tidak fleksibel         | Menggunakan `layout.js`  |
| Routing Dinamis         | `pages/[id].js`         | `app/[id]/page.js`       |

## Kapan Menggunakan Page Router atau App Router?

- **Page Router**: Cocok jika masih menggunakan versi Next.js sebelum 13 atau aplikasi yang tidak memerlukan fitur-fitur baru seperti streaming dan nested layouts.
- **App Router**: Disarankan untuk aplikasi yang menggunakan Next.js 13 atau lebih tinggi, terutama jika ingin memanfaatkan keunggulan Server Components dan fitur data fetching yang lebih fleksibel.


# Pembuatan Routes dalam App Router

Next.js versi 13 memperkenalkan **App Router** baru yang dibangun di atas *React Server Components*, yang mendukung fitur seperti shared layouts, nested routing (routing bertingkat), loading states, error handling, dan lainnya.

App Router bekerja di dalam direktori baru bernama `app`. Direktori `app` ini dapat bekerja berdampingan dengan direktori `pages`, sehingga memungkinkan adopsi bertahap. Dengan demikian, Anda dapat mengaktifkan beberapa rute aplikasi menggunakan perilaku baru, sambil mempertahankan rute lainnya di direktori `pages` dengan perilaku lama. Jika aplikasi Anda menggunakan direktori `pages`.

> **Penting**: App Router memiliki prioritas lebih tinggi daripada Pages Router. Rute pada direktori yang berbeda seharusnya tidak mengarah ke jalur URL yang sama, karena ini akan menyebabkan error saat proses build untuk mencegah konflik.

Secara default, *React Server Components* adalah komponen dalam `app`. Hal ini dirancang untuk meningkatkan kinerja dan memudahkan penggunaan. Selain itu, Anda juga dapat menggunakan *Client Components*.

## Pembuatan Routes

App Router di Next.js memungkinkan pembuatan rute dalam aplikasi dengan mudah melalui struktur direktori `app/`. Sistem ini memanfaatkan struktur file secara otomatis, sehingga konfigurasi rute manual tidak diperlukan. Setiap folder dan file dalam `app/` secara langsung diubah menjadi rute, dengan nama file sebagai nama rute dan komponen yang diekspor sebagai halaman yang ditampilkan.

### Struktur Dasar

Pada App Router, kita menggunakan folder `app/` untuk menentukan rute aplikasi. Struktur file dan foldernya dapat berupa seperti ini:

```plaintext
app/
 ├── page.tsx           // Rute utama "/"
 ├── about/
 │   └── page.tsx       // Rute "/about"
 └── blog/
     ├── page.tsx       // Rute "/blog"
     └── [slug]/
         └── page.tsx   // Rute dinamis "/blog/[slug]"
```

Setiap folder dan file `page.tsx` dalam folder `app/` langsung menjadi rute berdasarkan struktur folder tersebut. Menggunakan nama folder dengan format `[parameter]` memungkinkan kita membuat rute dinamis untuk menerima parameter URL.

### Implementasi

Untuk membuat rute sederhana dalam App Router, ikuti langkah-langkah berikut:

1. Buat rute utama dengan membuat file `page.tsx` dalam folder `app/`.

    ```javascript
    // app/page.tsx
    const HomePage = () => {
        return <div>Welcome to the Homepage!</div>;
    };

    export default HomePage;
    ```

2. Tambahkan rute lain, seperti rute `/about`, dengan menambahkan sub-folder `about` dan file `page.tsx`.

    ```javascript
    // app/about/page.tsx
    const AboutPage = () => {
        return <div>About Us</div>;
    };

    export default AboutPage;
    ```

3. Tambahkan rute dinamis, misalnya `/blog/[slug]`, dengan membuat folder `[slug]` di dalam `app/blog`.

    ```javascript
    // app/blog/[slug]/page.tsx
    const BlogPost = ({ params }) => {
        return <div>Blog Post: {params.slug}</div>;
    };

    export default BlogPost;
    ```

4. Jalankan server lokal dengan perintah berikut:

    ```bash
    npm run dev
    ```

Sekarang, Anda dapat menavigasi ke URL yang sesuai (misalnya, `/about` atau `/blog/my-first-post`) dan melihat halaman yang telah dibuat.

## Fitur pada App Router

- **Route Otomatis**: Membuat file `page.tsx` atau `page.jsx` di dalam direktori `app/` otomatis menghasilkan rute yang sesuai.
- **Komponen Layout yang Konsisten**: Setiap folder dapat memiliki file `layout.tsx` untuk membuat layout yang konsisten di seluruh rute dalam folder tersebut.
- **Data Fetching Lebih Fleksibel**: Pengambilan data (data fetching) dapat dilakukan langsung dalam komponen.

## Kelebihan

- **Efisiensi**: Membuat rute lebih mudah dan cepat tanpa konfigurasi manual.
- **Dukungan Server-Side Components**: Secara default, setiap komponen di dalam App Router adalah Server Component.
- **Struktur Layout yang Lebih Kompleks**: File `layout.tsx` memungkinkan pengaturan layout global di seluruh aplikasi.

## Kekurangan

- **Rute yang Kompleks**: Untuk aplikasi dengan banyak rute dinamis dan bertingkat, struktur file dapat menjadi kompleks.
- **Keterbatasan Client-Side Components**: Hanya komponen yang secara eksplisit diatur sebagai client yang dapat menggunakan interaktivitas dan event listeners, sementara komponen lainnya default-nya adalah server-side.

Dengan App Router, Next.js menyediakan sistem routing yang fleksibel dan efisien, menjadikannya cocok untuk berbagai jenis aplikasi dari yang sederhana hingga yang kompleks.

# Layout

Isi di sini.