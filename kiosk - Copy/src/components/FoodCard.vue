<template>

    <div class="card p-2 bg-pink m-1 mb-2">
          
        <div class="d-flex justify-content-between">
            <p>{{ Name }}</p>
            <p>P{{Price}}</p>
        </div>

        <img class="mt-2 mb-2" src="https://images.squarespace-cdn.com/content/v1/57879a6cbebafb879f256735/1579721909133-R2KSZ8VGDGBI90DYATBK/header4.jpg" alt="">

        <div class="d-flex justify-content-between">
            <button class="btn btn-danger border">-</button>
            <p>0</p>
            <button class="btn bg-success border">+</button>
        </div>

    </div>

</template>

<script>
export default {
  props: {
    Name: String,
    Price: Number
  }
}
</script>

