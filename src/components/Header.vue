<template>
    <div>
        <b-card title="Card Title" no-body>
            <b-card-header header-tag="nav">
            <b-nav card-header pills>
                <b-nav-item disabled class="title">Quiz App</b-nav-item>
                <b-nav-item disabled>Counter {{numCorrect}}/{{numTotal}}</b-nav-item>
            </b-nav>
            </b-card-header>
        </b-card>
    </div>
</template>

<script>
export default {
    name:"Header",
    props:[
        'numCorrect',
        'numTotal'
    ]
}
</script>

<style scoped>
.title{
	font-weight: bolder;
}

</style>
