<template>
    <div class="container">
        <div class="row">
            <table class="table table-bordered">
                <thead>
                    <tr>
                        <th scope="col" class="text-center table-primary">번호</th>
                        <td scope="col" class="text-center">{{ boardInfo.no }}</td>
                        <th scope="col" class="text-center table-primary">작성일</th>
                        <td scope="col" class="text-center">{{ getDateFormat(boardInfo.created_date) }}</td>
                        <th scope="col" class="text-center table-primary">이름</th>
                        <td scope="col" class="text-center">{{ boardInfo.writer }}</td>
                    </tr>

                    <tr>
                        <th colspan="2" class="text-center table-primary">제목</th>
                        <td colspan="4">{{ boardInfo.title }}</td>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td colspan="6" class="text-left" valign="top" height="200">
                            <pre
                                style="white-space: pre-wrap;border:none;background-color: white;">{{ boardInfo.content }}</pre>
                        </td>
                    </tr>
                    <tr>
                        <td colspan="6" class="text-center">
                            <button class="btn btn-xs btn-info" @click="goToUpdateForm(boardInfo.no)">수정</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="row">
            <!-- 해당 게시글의 댓글이 있다면 -->
            <CommentList v-if="boardInfo.comment> 0" 
                        v-bind:bno="boardInfo.no"/>
            <!-- 해당 게시글의 댓글이 있다면 -->
            <div v-else class="card text-center">
                댓글없음
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
import CommentList from '../components/CommentList.vue';

export default {
    components: { 
        CommentList
    },
    data() {
        return {
            searchNo: '',
            boardInfo: {}
        };
    },
    created() {
        // boardList.vue 에서 info는 라우터 쿼리로 보내서 여기서 searchNo로 받음
        this.searchNo = this.$route.query.boardNo;  // page에서 요청한 정보는 rourer임
        this.getBoardInfo();
    },
    methods: {
        async getBoardInfo() {
            // 받아올때 까지 기다림 (await)  axios는 어떤 기능을 수행해달라 요청
            let result = await axios.get(`/api/boards/${this.searchNo}`)
                                    .catch(err => console.log(err));
            this.boardInfo = result.data;
        
        },
        getDateFormat(date) {
            return this.$dateFormat(date);
        },
        goToUpdateForm(no) {
            this.$router.push({path: '/form', query : { boardNo : no }});
        }
    }
}
</script>
