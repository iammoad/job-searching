<template>
  <section class="content container">
        <h1 class="title">you show {{ itemsCount }} element</h1>
        <div class="main-content">

            <template v-if="itemsList.length">
                <div 
                    class="item"
                    v-for="item in itemsList"
                    :key="item.id">
                    <div class="item-header">
                        <div class="image">
                            <img class="logo__image" :src="item.companyImage" alt="logo">
                        </div>
                        <div class="company-info">
                            <div class="company-name">{{item.jobCompany}}</div>
                            <div class="company-location">{{item.jobLocation}}</div>
                        </div>
                    </div>
                    <div class="item-title">
                        {{ item.jobTitle}}
                    </div>
                    <p class="item-decription">
                        {{ item.jobDecription }}
                    </p>
                    <div class="item-buttons">
                        <button class="apply btn btn--primary">apply now</button>
                        <button class="message btn btn--secondary">message</button>
                    </div>
                </div>
            </template>
            <div v-else>
                no job found
            </div>
        </div>
    </section>
</template>

<script>
import {ref, computed } from 'vue'
import { useStore } from 'vuex'
export default {
    name : 'JobsList',
    setup(){
        const itemsCount = ref('');
        const store = useStore();
        store.dispatch('getJObs');
        const itemsList = computed(() => {
                    const data = store.getters.getJobsList;
                    return data.filter(item => {
                        console.log(item);
                        const a= item.jobTitle.includes(store.getters.getSearchedTitle);
                        const b = item.jobCompany.includes(store.getters.getSearchedCompany) ;
                        const c= item.jobLocation.includes(store.getters.getSearchedLocation) ;
                        return ( a && c && b);
                    });
                }
        );
        itemsCount.value = computed(() => itemsList.value.length);
        return{
            itemsCount,
            itemsList
        }
    },
}
</script>
<style lang="scss" scoped>
.content{
    color: #fff;
    padding: 1em 0;
    .title{
        text-align: center;
        text-transform: uppercase;
        font-size: .9em;
    }
    .main-content{
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 1.8em;
        padding: 2em 0;
    }
}

 .item{
            border-radius: 10px;
            background-color: #1C1C24;
            box-shadow: 0px 0px 1px darken($color: #1C1C24, $amount: 4);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 2em 1em;
            cursor: pointer;
            transition: all .3s ease;
            min-width: 200px;
            max-width: 350px;
            &:hover{
                transform: scale(1.02);
            }
            .item-header{
                display: flex;
                gap: 1em;
                align-items: center;
                margin-bottom: 1em;
                .image{
                    overflow: hidden;
                    img{
                        width: 60px;
                        height: 60px;
                        border-radius: 50%;
                    }
                }
                .company-info{
                    display: flex;
                    flex-direction: column;
                    gap: .5em;
                    .company-name{
                        font-weight: 600;
                        text-transform: uppercase;
                    }
                    .company-location{
                        color: whitesmoke;
                    }
                }
            }
            .item-title{
                font-size: 1.4em;
                font-weight: 600;
                text-transform: capitalize;
            }
            .item-decription{
                line-height: 1.4;
                letter-spacing: .03em;
                font-weight: 600;
                color: #96939B;
                max-height: 160px;
                overflow: hidden;
                text-overflow: ellipsis;
            }
            .item-buttons{
                width: 100%;
                display: flex;
                justify-content: space-between;
            }
        }
</style>