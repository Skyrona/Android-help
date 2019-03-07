# AsyncTask

Cette classe permet de réaliser des opérations en background et de publier les résultats dans l'UI.

Idéalement **AsyncTasks** devrait être utilisé pour des opérations courtes (quelques secondes au plus). Pour des opérations plus longues il sera recommandé d'utiliser les diverses API fournit par le **java.util.concurrent** package tels que **Executor**, **ThreadPoolExecutor** and **FutureTask**.

Une tâche asynchrone est définie par un calcul qui tourne en background et dont le résultat est publié dans l'UI thread. Une tâche asynchrone est définie par trois types génériques appellés : **Params**, **Progress** and **Result** ainsi que quatre étapes : **onPreExecute**, **doInBackground**, **onProgressUpdate** and **onPostExecute**.

Pour être utilisé, **AsyncTask** doit être sous-classé.

Cette sous-classe va surcharger au moins une méthode (**doInBackground(Params...)**) et plus souvent surchargera une seconde (**onPostExecute(Result)**).

```java
private class DownloadFilesTask extends AsyncTask<URL, Integer, Long>{

    protected Long doInBackground(URL... urls) {
        int count = urls.length;
        long totalSize = 0;
        for (int i = 0; i < count; i++) {
            totalSize += Downloader.downloadFile(urls[i]);
            publishProgress((int) ((i / (float) count) * 100));
            // Escape early if cancel() is called
            if (isCancelled()) break;
        }
        return totalSize;
    }

    protected void onProgressUpdate(Integer... progress) {
        setProgressPercent(progress[0]);
    }

    protected void onPostExecute(Long result) {
        showDialog("Downloaded " + result + " bytes");
    }

}
```

## Types génériques de AsyncTask

* **Params** : le type des paramètres envoyés à la tâche à l'exécution
* **Progress** : le type des unités de progrès publiés pendant le calcul en background
* **Result** : le type des résultats

Tous les types ne sont pas utilisés pendant une tâche asynchrone, à ce moment on utilise le type **Void**

```java
private class MyTask extends AsyncTask<Void, Void, Void> { ... }
```

## Etapes de AsyncTask

* **onPreExecute()** : invoqué dans le UI thread avant que la tâche soit exécuté. Elle sert à préparer la tâche, par exemple en montrant une bar de progrès dans l'UI.
* **doInBackground(Params...)** : invoqué dans le background thread immédiatement après que **onPreExecute()** ait fini son exécution. Les paramètres de la tâche asynchrone sont passés à cette étape. Le résultat du calcul doit être renvoyé par cette étape et sera passé à la dernière étape.
* **onProgressUpdate(Progress...)** : invoqué dans l'UI thread après un appel à **publishProgress(Progress...)**. Cette méthode est utilisée pour afficher une forme de progrès dans l'UI pendant que le calcul est en cours d'exécution.
* **onPostExecute(Result)** : invoqué dans l'UI thread après que le calcul ait fini. Le résultat du calcul de background est passé à cette étape comme paramètre.

## Annuler une tâche

Une tâche peut être annulée à tout moment en invoquant **cancel(boolean)**. Invoquer cette méthode cause un appel vers **isCancelled()** de renvoyer **true**.
Après **doInBackground(Object[])**, la méthode **onCancelled(Object)** sera appellée. Pour s'assurer qu'une tâche est annulée le plus vite possible, il faut vérifier le plus souvent possible la valeur de retour de **isCancelled()** (dans une loop par exemple).

## Faire fonctionner la classe

* La classe doit être chargée dans le UI thread.
* L'instance de la tâche doit être crée dans l'UI thread.
* **execute(Params...)** doit être invoquée dans l'UI thread.
* Ne pas appeler **onPreExecute()**, **onPostExecute(Result)**, **doInBackground(Params...)**, **onProgressUpdate(Progress...)** manuellement.
* La tâche peut être exécuté qu'une seule fois (une exception sera lancée si une seconde exécution est tentée).
