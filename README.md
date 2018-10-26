# Initial page

### Test

这是一个测试的例子，为了了解整个github和gitbook同步的区别

```c
long long int sum_simd(unsigned int vals[NUM_ELEMS]) {
	clock_t start = clock();
	__m128i _127 = _mm_set1_epi32(127);		// This is a vector with 127s in it... Why might you need this?
	long long int result = 0;				// This is where you should put your final result!
											// DO NOT DO NOT DO NOT DO NOT WRITE ANYTHING ABOVE THIS LINE.
	for(unsigned int w = 0; w < OUTER_ITERATIONS; w++) {
		/* YOUR CODE GOES HERE */
		unsigned int sum_by_four[4];
		__m128i all_zeros =  _mm_setzero_si128();
		unsigned int i = 0;
		for(; i< NUM_ELEMS - 4; i+=4)
		{
			__m128i _four = _mm_loadu_si128((__m128i*)(vals + i));
			__m128i compare = _mm_cmpgt_epi32(_four, _127); 
			_four = _mm_and_si128(_four, compare);
			all_zeros = _mm_add_epi32(_four, all_zeros);
		}

		_mm_storeu_si128((__m128i*)sum_by_four, all_zeros);

		for(unsigned int j = 0; j<4 ; j++)
		{
			result += sum_by_four[j];	
		}
		for(; i < NUM_ELEMS ; i++)
		{
			if(vals[i] > 127)
				result += vals[i];
		}
		/* You'll need a tail case. */
	}
	clock_t end = clock();
	printf("Time taken: %Lf s\n", (long double)(end - start) / CLOCKS_PER_SEC);
	return result;
}
```

