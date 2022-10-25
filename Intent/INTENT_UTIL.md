### Pening Intent
```
private val geofenceIntent: PendingIntent by lazy {
        val intent = Intent(context, GeofenceBroadcastReceiver::class.java)
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
            PendingIntent.getBroadcast(context, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT or PendingIntent.FLAG_MUTABLE)
        } else {
            PendingIntent.getBroadcast(context, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT)
        }
    }
```
TODO: Provide explanation

### Choose To Get Image From Gallery or Taking A Picture
```
fun imageActionChooser(context: Context, description: String) {
    val galleryIntent = Intent().apply {
        action = Intent.ACTION_GET_CONTENT 
        type = "image/*" //defines what image to get
        flags = Intent.FLAG_ACTIVITY_NEW_TASK
    }

    val cameraIntent = Intent().apply {
        action = MediaStore.ACTION_IMAGE_CAPTURE
    }

    val chooseIntent = Intent.createChooser(galleryIntent, description).apply { //description is used to add a description for the bottom sheet that   
        putExtra(Intent.EXTRA_INITIAL_INTENTS, arrayOf(cameraIntent))           //will show up
    }

    context.startActivity(chooseIntent)
}
```

