# Opencv Android

Ce repo contient le code d'OpenCV pour Android. Le seul but de ce repo est de permettre de publier OpenCV en tant que librairie, qui peut être importée dans n'importe quel autre projet Android.  
La librairie est publié sur S3, sur `s3://cornis-maps-dev`. **Il faut donc y avoir accès pour pouvoir publier**.

## Que fait ce repo ?

La [documentation officielle d'OpenCV](https://docs.opencv.org/4.x/d5/df8/tutorial_dev_with_OCV_on_Android.html) recommande de télécharger manuellement la release, et de l'importer en tant que nouveau module. C'est pas super, parce que ça pollue beaucoup le repo, en ajoutant des centaines de fichiers. J'espère que cette recommendation changera dans le futur (état fin 2025), mais en attendant ce repo permet d'importer la librairie de façon plus standard.


## Publier

Il suffit de faire :
```
./gradlew publishAllPublicationsToS3-repoRepository
```

La configuration de la publication est située dans [opencv/build.gradle)](opencv/build.gradle). Voilà à quoi elle ressemble:
```kotlin
repositories {
    maven {
        name = 's3-repo'
        url = "s3://cornis-maps-dev/android/OpenCV-Android/maven-repository"
        authentication {
            awsIm(AwsImAuthentication)
        }
    }
}
```


## Utiliser la librairie dans un autre projet

Pour l'importer, il suffit de configurer Gradle pour qu'il aille chercher sur S3. Dans `settings.gradle.kts`:
```kotlin
repositories {
    // ... other repos
    maven {
        name = 's3-repo'
        url = "s3://cornis-maps-dev/android/OpenCV-Android/maven-repository"
        authentication {
            awsIm(AwsImAuthentication)
        }
    }
}
```

Puis ajouter dans le `build.gradle.kts` du module qui en a besoin:
```kotlin
implementation("com.cornis:opencv:4.12.0")
```
