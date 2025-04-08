
Esranur Ulubaba <esranur.ulubaba@gmail.com>
13:37 (0 dakika önce)
Alıcı: ben

1.Önce githubta repo aç 2. Sonra repo linkini al vscode a gel . Clone repository yap ve linki yapıştır . Daha önce açtığın içi boş klasör ile oraya bağlan. 3.Açılan projenin içine bir tane index.html aç 4.Daha sonra commit ve commit all yaparak ilk commiti yap. 5.Bir Dockerfile dosyası aç ve içini doldur sonra onu da commitle. FROM nginx:latest

COPY . /usr/share/nginx/html

EXPOSE 80

Kodun olduğu yere gel ve terminali aç . Terminalde ; docker build –t imageadı .

Dockeri aç ve image yerinden port vererek çalıştır. 8.CContainer yerine gel oluşan konteynerin localhost kısmına tıkla orada sayfaya gidecek. 9.Sonra hepsini sildi.

Jenkinse gel ve bir pipeline aç . Github Project seç ve projenin url sini yapıştır. Github hook trigger seç Pipeline script yerine gel ve şu kodu yapıştır. pipeline { agent any

triggers { githubPush() }

environment { IMAGE_NAME="ymg-ders-test" CONTAINER_NAME="nginx-test-conteyner" }

stages { stage('Repoyu Klonla') { steps { git url: 'https://github.com/02230224093/YMGDersOrnek.git', branch: 'main' } }

stage('Docker Image Oluştur') {
    steps {
        echo "Docker Image oluşturuluyor..."
        bat "docker build -t %IMAGE_NAME% ."
    }
}

stage('Eski Conteyneri Durdur') {
    steps {
        echo "Eski konteyner durduruluyor..."
        bat "docker rm -f %CONTAINER_NAME% || exit 0"
    }
}

stage('Yeni Conteyneri Oluştur') {
    steps {
        echo "Yeni konteyner oluşturuluyor..."
        bat "docker run -d --name %CONTAINER_NAME% -p 5050:80 %IMAGE_NAME%"
    }
}
}

post { success { echo "Yayın başarılı: http://localhost:5050" } failure { echo "Pipeline başarısız oldu" } } } İçinde düzenlemeler yap.

Şimdi yapılandıra bas ve çalıştır daha sonra localhost verdiğin porta gidip bak. 12.Ngrok orda olan tokeni al reponun ayarlar yerine git , github webhook yap payload url yerine yapıştır .

Artık pipelineyi kendisi yapılandırıp çalıştıracak.
