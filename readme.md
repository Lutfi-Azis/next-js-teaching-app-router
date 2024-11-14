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

Isi di sini.

# Pembuatan Routes dalam App Router

Isi di sini.

# Layout

Isi di sini.