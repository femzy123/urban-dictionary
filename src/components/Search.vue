<template>
    <div>
        <div class="input-group col-12">
            <input type="text" class="form-control" placeholder="Search for a word" />
            <span class="input-group-btn">
                <button class="btn btn-primary" type="button">
                    Search
                </button>
            </span>
        </div>
    </div>
</template>

<script>
export default {
    name: "Search",
    data() {
        return {
            text: ""
        }
    }
}
</script>

<style>

</style>
