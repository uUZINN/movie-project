<template>
    <!-- 컴포넌트의 템플릿 -->
    <div>
        <!-- movieId를 여기서 사용합니다 -->
        <h1>영화 상세 정보 - {{ movieId }}</h1>
        <!-- 영화 상세 정보와 관련된 다른 내용들 -->
    </div>
</template>
  
<script>
export default {
    name: 'DetailView',
    props: {
        movieId: {
            type: String, // movieId의 실제 유형에 맞게 타입 변경
            required: true // 필요한 경우 movieId를 필수로 표시
        }
        // 필요한 경우 다른 props 추가
    },
    // 다른 컴포넌트 로직
};
</script>